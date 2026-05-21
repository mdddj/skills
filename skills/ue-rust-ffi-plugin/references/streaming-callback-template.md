# Streaming Callback Template

Use this when Rust streams partial results to UE, such as AI token-by-token output, progress events, logs, audio chunks, or long-running task updates.

## Design Rules

- Use a C ABI callback with `user_data: *mut c_void`.
- Treat callback buffers as borrowed and short-lived. UE must copy bytes before returning from the callback.
- Do not call `UObject`, Slate, UMG, Blueprint delegates, or gameplay state directly from a Rust callback thread. Use `AsyncTask(ENamedThreads::GameThread, ...)`.
- Return an opaque Rust handle for long-running streams so UE can cancel/free it.
- Make cancellation idempotent. It should be safe to call cancel during stream completion.
- Catch Rust panics before crossing FFI.
- Keep callbacks non-blocking. If UE work is heavy, enqueue it and return.

## C ABI Shape

```rust
use std::ffi::{c_char, c_void, CStr, CString};
use std::os::raw::c_int;
use std::sync::{
    atomic::{AtomicBool, Ordering},
    Arc,
};
use std::thread::{self, JoinHandle};

pub type UeStreamTokenCallback =
    extern "C" fn(user_data: *mut c_void, data: *const c_char, len: usize);

pub type UeStreamDoneCallback =
    extern "C" fn(user_data: *mut c_void, code: c_int);

pub struct UeAiStreamHandle {
    cancel: Arc<AtomicBool>,
    worker: Option<JoinHandle<()>>,
}

#[no_mangle]
pub extern "C" fn ue_ai_stream_start(
    prompt: *const c_char,
    user_data: *mut c_void,
    on_token: Option<UeStreamTokenCallback>,
    on_done: Option<UeStreamDoneCallback>,
) -> *mut UeAiStreamHandle {
    let result = std::panic::catch_unwind(|| {
        if prompt.is_null() {
            return std::ptr::null_mut();
        }

        let prompt = unsafe { CStr::from_ptr(prompt) }
            .to_string_lossy()
            .into_owned();

        let user_data_bits = user_data as usize;
        let cancel = Arc::new(AtomicBool::new(false));
        let worker_cancel = Arc::clone(&cancel);

        let worker = thread::spawn(move || {
            let user_data = user_data_bits as *mut c_void;

            // Replace this demo loop with the real AI/network stream.
            for chunk in fake_ai_stream(prompt) {
                if worker_cancel.load(Ordering::Relaxed) {
                    if let Some(done) = on_done {
                        done(user_data, 1); // cancelled
                    }
                    return;
                }

                if let Some(callback) = on_token {
                    let sanitized = chunk.replace('\0', "");
                    if let Ok(c_chunk) = CString::new(sanitized) {
                        let bytes = c_chunk.as_bytes();
                        callback(user_data, c_chunk.as_ptr(), bytes.len());
                        // c_chunk is dropped here. UE must copy before callback returns.
                    }
                }
            }

            if let Some(done) = on_done {
                done(user_data, 0); // success
            }
        });

        Box::into_raw(Box::new(UeAiStreamHandle {
            cancel,
            worker: Some(worker),
        }))
    });

    result.unwrap_or(std::ptr::null_mut())
}

#[no_mangle]
pub extern "C" fn ue_ai_stream_cancel(handle: *mut UeAiStreamHandle) {
    if handle.is_null() {
        return;
    }

    let handle = unsafe { &*handle };
    handle.cancel.store(true, Ordering::Relaxed);
}

#[no_mangle]
pub extern "C" fn ue_ai_stream_free(handle: *mut UeAiStreamHandle) {
    if handle.is_null() {
        return;
    }

    let mut handle = unsafe { Box::from_raw(handle) };
    handle.cancel.store(true, Ordering::Relaxed);

    if let Some(worker) = handle.worker.take() {
        let _ = worker.join();
    }
}

fn fake_ai_stream(prompt: String) -> impl Iterator<Item = String> {
    format!("Echo: {prompt}").chars().map(|ch| ch.to_string()).collect::<Vec<_>>().into_iter()
}
```

For Rust 2024 edition, use `#[unsafe(no_mangle)]`.

## Important Rust Send/Thread Caveat

Do not move raw `*mut c_void` directly into `thread::spawn`; raw pointers are not a good cross-thread ownership signal and may fail `Send` bounds. Store the pointer bits as an integer inside the thread boundary and cast it back only when invoking callbacks:

```rust
let user_data_bits = user_data as usize;

let worker = thread::spawn(move || {
    let user_data = user_data_bits as *mut c_void;
    // call callback(user_data, ...)
});
```

Only do this when UE guarantees the pointed object stays alive until `done`, `cancel/free`, or both complete. A stronger design stores a heap-allocated C++ wrapper whose lifetime owns the Rust handle.

## UE C++ Callback Wrapper

```cpp
// Public/MyAiStream.h
#pragma once

#include "CoreMinimal.h"
#include "UObject/Object.h"
#include "MyAiStream.generated.h"

DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FRustAiTokenDelegate, const FString&, Token);
DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FRustAiDoneDelegate, int32, Code);

UCLASS(BlueprintType)
class MYPLUGIN_API UMyAiStream : public UObject
{
    GENERATED_BODY()

public:
    UPROPERTY(BlueprintAssignable)
    FRustAiTokenDelegate OnToken;

    UPROPERTY(BlueprintAssignable)
    FRustAiDoneDelegate OnDone;

    UFUNCTION(BlueprintCallable)
    bool Start(const FString& Prompt);

    UFUNCTION(BlueprintCallable)
    void Cancel();

    virtual void BeginDestroy() override;

private:
    void* Handle = nullptr;

    static void HandleToken(void* UserData, const char* Data, size_t Len);
    static void HandleDone(void* UserData, int Code);
};
```

```cpp
// Private/MyAiStream.cpp
#include "MyAiStream.h"

#include "Async/Async.h"

#include <string>

THIRD_PARTY_INCLUDES_START
#include "ue_ai_stream_bridge.h"
THIRD_PARTY_INCLUDES_END

bool UMyAiStream::Start(const FString& Prompt)
{
    if (Handle != nullptr)
    {
        return false;
    }

    Handle = ue_ai_stream_start(
        TCHAR_TO_UTF8(*Prompt),
        this,
        &UMyAiStream::HandleToken,
        &UMyAiStream::HandleDone);

    return Handle != nullptr;
}

void UMyAiStream::Cancel()
{
    if (Handle != nullptr)
    {
        ue_ai_stream_cancel(static_cast<UeAiStreamHandle*>(Handle));
    }
}

void UMyAiStream::BeginDestroy()
{
    if (Handle != nullptr)
    {
        ue_ai_stream_free(static_cast<UeAiStreamHandle*>(Handle));
        Handle = nullptr;
    }

    Super::BeginDestroy();
}

void UMyAiStream::HandleToken(void* UserData, const char* Data, size_t Len)
{
    UMyAiStream* Stream = static_cast<UMyAiStream*>(UserData);
    if (Stream == nullptr || Data == nullptr)
    {
        return;
    }

    const std::string Copy(Data, Len);
    const FString Token = UTF8_TO_TCHAR(Copy.c_str());

    AsyncTask(ENamedThreads::GameThread, [WeakStream = TWeakObjectPtr<UMyAiStream>(Stream), Token]()
    {
        if (UMyAiStream* StreamOnGameThread = WeakStream.Get())
        {
            StreamOnGameThread->OnToken.Broadcast(Token);
        }
    });
}

void UMyAiStream::HandleDone(void* UserData, int Code)
{
    UMyAiStream* Stream = static_cast<UMyAiStream*>(UserData);
    if (Stream == nullptr)
    {
        return;
    }

    AsyncTask(ENamedThreads::GameThread, [WeakStream = TWeakObjectPtr<UMyAiStream>(Stream), Code]()
    {
        if (UMyAiStream* StreamOnGameThread = WeakStream.Get())
        {
            void* CompletedHandle = StreamOnGameThread->Handle;
            StreamOnGameThread->Handle = nullptr;

            if (CompletedHandle != nullptr)
            {
                ue_ai_stream_free(static_cast<UeAiStreamHandle*>(CompletedHandle));
            }

            StreamOnGameThread->OnDone.Broadcast(Code);
        }
    });
}
```

## Lifetime Warning

The example passes `this` as `user_data`. That is acceptable only if the `UObject` stays alive while Rust may call callbacks. For production:

- Keep the `UObject` referenced by the caller until `OnDone`.
- Cancel/free in `BeginDestroy`.
- Prefer a separate heap-allocated native context if callbacks may outlive a UObject.
- Avoid joining a Rust worker on the game thread if the worker can block on network I/O. Prefer cooperative cancellation plus a non-blocking free or background cleanup for real AI clients.
- Do not call a Rust `free` function that joins the worker from inside the Rust worker thread itself. The example schedules free back to the UE game thread after `OnDone`.

## Status Codes

Use explicit status codes across the ABI:

```text
0  success
1  cancelled
-1 invalid argument
-2 network/client error
-3 panic caught at FFI boundary
```

Send error text through a separate callback if the user needs detailed diagnostics:

```rust
pub type UeStreamErrorCallback =
    extern "C" fn(user_data: *mut c_void, data: *const c_char, len: usize, code: c_int);
```
