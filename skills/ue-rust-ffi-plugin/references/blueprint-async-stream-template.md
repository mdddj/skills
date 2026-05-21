# Blueprint Async Stream Template

Use this for the most UE-friendly surface over a Rust streaming FFI API. The Rust ABI still uses `start / cancel / free + callbacks`, but gameplay and Blueprint users see a single async node:

```text
Stream AI From Rust
  OnToken(Token)
  OnError(Code, Message)
  OnDone(Code)
```

This is usually cleaner than exposing a raw `UObject` stream wrapper to Blueprints.

Use `ExposedAsyncProxy` when Blueprint users need to cancel the stream manually. It exposes the async action proxy object pin so Blueprint can call `Cancel()`.

## Build.cs Requirements

The module that declares `UBlueprintAsyncActionBase` must depend on `Engine`.

```csharp
PublicDependencyModuleNames.AddRange(new[]
{
    "Core",
    "CoreUObject",
    "Engine"
});
```

If the async node lives in a runtime plugin module, do not add editor-only dependencies such as `UnrealEd`.

## Header

```cpp
// Public/RustAiStreamAsyncAction.h
#pragma once

#include "CoreMinimal.h"
#include "Kismet/BlueprintAsyncActionBase.h"
#include "RustAiStreamAsyncAction.generated.h"

DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FRustAiStreamToken, const FString&, Token);
DECLARE_DYNAMIC_MULTICAST_DELEGATE_TwoParams(FRustAiStreamError, int32, Code, const FString&, Message);
DECLARE_DYNAMIC_MULTICAST_DELEGATE_OneParam(FRustAiStreamDone, int32, Code);

struct FRustAiStreamCallbackContext;

UCLASS(BlueprintType, meta = (ExposedAsyncProxy = "AsyncTask"))
class MYPLUGIN_API URustAiStreamAsyncAction : public UBlueprintAsyncActionBase
{
    GENERATED_BODY()

public:
    UPROPERTY(BlueprintAssignable)
    FRustAiStreamToken OnToken;

    UPROPERTY(BlueprintAssignable)
    FRustAiStreamError OnError;

    UPROPERTY(BlueprintAssignable)
    FRustAiStreamDone OnDone;

    UFUNCTION(BlueprintCallable, meta = (BlueprintInternalUseOnly = "true", WorldContext = "WorldContextObject", DisplayName = "Stream AI From Rust"), Category = "Rust AI")
    static URustAiStreamAsyncAction* StreamAiFromRust(UObject* WorldContextObject, const FString& Prompt);

    UFUNCTION(BlueprintCallable, Category = "Rust AI")
    void Cancel();

    virtual void Activate() override;
    virtual void BeginDestroy() override;

private:
    UPROPERTY()
    TObjectPtr<UObject> WorldContextObject;

    FString Prompt;
    void* StreamHandle = nullptr;
    FRustAiStreamCallbackContext* CallbackContext = nullptr;
    bool bFinished = false;

    void Finish(int32 Code);
    void Fail(int32 Code, const FString& Message);
    void FreeStreamHandle();

    static void HandleToken(void* UserData, const char* Data, size_t Len);
    static void HandleError(void* UserData, const char* Data, size_t Len, int Code);
    static void HandleDone(void* UserData, int Code);
};
```

Replace `MYPLUGIN_API` with the actual module API macro.

## Implementation

```cpp
// Private/RustAiStreamAsyncAction.cpp
#include "RustAiStreamAsyncAction.h"

#include "Async/Async.h"
#include "Engine/Engine.h"

#include <string>

THIRD_PARTY_INCLUDES_START
#include "ue_ai_stream_bridge.h"
THIRD_PARTY_INCLUDES_END

struct FRustAiStreamCallbackContext
{
    TWeakObjectPtr<URustAiStreamAsyncAction> Action;
};

URustAiStreamAsyncAction* URustAiStreamAsyncAction::StreamAiFromRust(UObject* WorldContextObject, const FString& Prompt)
{
    URustAiStreamAsyncAction* Action = NewObject<URustAiStreamAsyncAction>();
    Action->WorldContextObject = WorldContextObject;
    Action->Prompt = Prompt;
    Action->RegisterWithGameInstance(WorldContextObject);
    return Action;
}

void URustAiStreamAsyncAction::Activate()
{
    if (Prompt.IsEmpty())
    {
        Fail(-1, TEXT("Prompt is empty."));
        return;
    }

    CallbackContext = new FRustAiStreamCallbackContext();
    CallbackContext->Action = this;

    StreamHandle = ue_ai_stream_start(
        TCHAR_TO_UTF8(*Prompt),
        CallbackContext,
        &URustAiStreamAsyncAction::HandleToken,
        &URustAiStreamAsyncAction::HandleDone);

    if (StreamHandle == nullptr)
    {
        Fail(-2, TEXT("Failed to start Rust AI stream."));
    }
}

void URustAiStreamAsyncAction::Cancel()
{
    if (StreamHandle != nullptr && !bFinished)
    {
        ue_ai_stream_cancel(static_cast<UeAiStreamHandle*>(StreamHandle));
    }
}

void URustAiStreamAsyncAction::BeginDestroy()
{
    Cancel();
    FreeStreamHandle();
    Super::BeginDestroy();
}

void URustAiStreamAsyncAction::Finish(int32 Code)
{
    if (bFinished)
    {
        return;
    }

    bFinished = true;
    FreeStreamHandle();
    OnDone.Broadcast(Code);
    SetReadyToDestroy();
}

void URustAiStreamAsyncAction::Fail(int32 Code, const FString& Message)
{
    if (bFinished)
    {
        return;
    }

    bFinished = true;
    FreeStreamHandle();
    OnError.Broadcast(Code, Message);
    OnDone.Broadcast(Code);
    SetReadyToDestroy();
}

void URustAiStreamAsyncAction::FreeStreamHandle()
{
    if (StreamHandle != nullptr)
    {
        void* HandleToFree = StreamHandle;
        StreamHandle = nullptr;
        ue_ai_stream_free(static_cast<UeAiStreamHandle*>(HandleToFree));
    }

    if (CallbackContext != nullptr)
    {
        delete CallbackContext;
        CallbackContext = nullptr;
    }
}

void URustAiStreamAsyncAction::HandleToken(void* UserData, const char* Data, size_t Len)
{
    FRustAiStreamCallbackContext* Context = static_cast<FRustAiStreamCallbackContext*>(UserData);
    if (Context == nullptr || Data == nullptr || Len == 0)
    {
        return;
    }

    const TWeakObjectPtr<URustAiStreamAsyncAction> WeakAction = Context->Action;
    const std::string Copy(Data, Len);
    const FString Token = UTF8_TO_TCHAR(Copy.c_str());

    AsyncTask(ENamedThreads::GameThread, [WeakAction, Token]()
    {
        if (URustAiStreamAsyncAction* ActionOnGameThread = WeakAction.Get())
        {
            if (!ActionOnGameThread->bFinished)
            {
                ActionOnGameThread->OnToken.Broadcast(Token);
            }
        }
    });
}

void URustAiStreamAsyncAction::HandleError(void* UserData, const char* Data, size_t Len, int Code)
{
    FRustAiStreamCallbackContext* Context = static_cast<FRustAiStreamCallbackContext*>(UserData);
    if (Context == nullptr)
    {
        return;
    }

    const TWeakObjectPtr<URustAiStreamAsyncAction> WeakAction = Context->Action;
    FString Message;
    if (Data != nullptr && Len > 0)
    {
        const std::string Copy(Data, Len);
        Message = UTF8_TO_TCHAR(Copy.c_str());
    }

    AsyncTask(ENamedThreads::GameThread, [WeakAction, Code, Message]()
    {
        if (URustAiStreamAsyncAction* ActionOnGameThread = WeakAction.Get())
        {
            ActionOnGameThread->Fail(Code, Message);
        }
    });
}

void URustAiStreamAsyncAction::HandleDone(void* UserData, int Code)
{
    FRustAiStreamCallbackContext* Context = static_cast<FRustAiStreamCallbackContext*>(UserData);
    if (Context == nullptr)
    {
        return;
    }

    const TWeakObjectPtr<URustAiStreamAsyncAction> WeakAction = Context->Action;

    AsyncTask(ENamedThreads::GameThread, [WeakAction, Code]()
    {
        if (URustAiStreamAsyncAction* ActionOnGameThread = WeakAction.Get())
        {
            if (Code == 0 || Code == 1)
            {
                ActionOnGameThread->Finish(Code);
            }
            else
            {
                ActionOnGameThread->Fail(Code, FString::Printf(TEXT("Rust stream failed with code %d."), Code));
            }
        }
    });
}
```

If the Rust ABI provides an error callback, pass `&URustAiStreamAsyncAction::HandleError` to `ue_ai_stream_start` and update the C header/signature accordingly.

## Rust ABI Alignment

The async action expects a Rust C ABI similar to:

```c
typedef void (*UeStreamTokenCallback)(void* user_data, const char* data, uintptr_t len);
typedef void (*UeStreamDoneCallback)(void* user_data, int code);

UeAiStreamHandle* ue_ai_stream_start(
    const char* prompt,
    void* user_data,
    UeStreamTokenCallback on_token,
    UeStreamDoneCallback on_done);

void ue_ai_stream_cancel(UeAiStreamHandle* handle);
void ue_ai_stream_free(UeAiStreamHandle* handle);
```

For richer errors:

```c
typedef void (*UeStreamErrorCallback)(void* user_data, const char* data, uintptr_t len, int code);
```

## Why This Is the Preferred UE Surface

- Blueprint users get one node and three exec-style event outputs through dynamic multicast delegates.
- The async action owns lifetime through `RegisterWithGameInstance` and `SetReadyToDestroy`.
- Low-level FFI handles stay private.
- Rust receives a native callback context rather than a raw UObject pointer.
- All callbacks are marshalled to the game thread before touching UObject state.
- C++ users can still use the lower-level wrapper when they need custom ownership or batching.

## Production Notes

- If `ue_ai_stream_free` can block on network shutdown, do not call it directly on the game thread. Add a Rust non-blocking detach/free variant or enqueue cleanup on a UE background task.
- If callers need explicit cancellation output, reserve status code `1` for cancelled and document it as a normal completion path.
- If multiple streams can run concurrently, each async action instance should own exactly one Rust handle.
- If the plugin can unload while streams are active, cancel/free all active streams in module shutdown before freeing the Rust dynamic library handle.
