---
name: ue-rust-ffi-plugin
description: Build or debug Unreal Engine 5 C++ plugins/modules that call Rust dynamic libraries through FFI. Use for UE Build.cs setup, cbindgen headers, Rust cdylib exports, extern "C" APIs, Windows Win64 DLL/import-library linking, macOS dylib loading, FPlatformProcess runtime loading, UTF-8 FString conversion, safe Rust-owned memory release, streaming callback bridges such as AI token-by-token responses, and Blueprint async action wrappers around Rust streams.
---

# UE Rust FFI Plugin

## Overview

Use this skill to turn a Rust library into a UE5-callable native dependency. Prefer a narrow C ABI between UE and Rust, then wrap that ABI in one UE C++ module/class so gameplay code never handles raw pointers directly.

This workflow covers three distinct integration modes:

- **Direct linked mode**: UE links the Rust import library/dylib and calls exported C functions directly.
- **Manual loaded mode**: UE loads the runtime library with `FPlatformProcess::GetDllHandle`, resolves functions with `GetDllExport`, and calls through function pointers.
- **Delay-load preload mode**: UE links an import library and uses delay-load, but preloads dependent DLLs/dylibs in module startup so the platform loader can resolve them reliably.

Do not silently change the integration mode. If a user asked for direct linked mode, do not switch to a subprocess/worker architecture unless the user explicitly approves that architecture change.

## Workflow

1. Inspect the UE plugin layout first.
   - Find `*.uplugin`, `Source/<Module>/<Module>.Build.cs`, `Public/`, `Private/`, `Binaries/<Platform>`, and `ThirdParty/<Lib>`.
   - Identify target platforms: usually `Mac`, `Win64`, or both.
   - Check whether Rust is a required startup dependency or optional runtime feature.
   - Check whether the code already uses direct calls, delay-load, or function pointers. Do not mix modes accidentally.

2. Choose the integration mode deliberately.
   - Use **direct linked mode** when loader behavior is simple and the library should be required at module load.
   - Use **manual loaded mode** for macOS or complex native stacks where hard-linking can cause editor startup hangs or symbol/dependency conflicts.
   - Use **delay-load preload mode** on Windows when UE should link an import library but dependent DLLs must be copied/preloaded from the plugin folder.
   - Use a subprocess/worker only as a separate, explicit architecture choice. Explain the tradeoff and get user approval first.

3. Build Rust as a `cdylib`.
   - Use `crate-type = ["cdylib"]`, not plain `dylib`.
   - Export only C ABI functions with `extern "C"` and `#[no_mangle]` or `#[unsafe(no_mangle)]`.
   - Never let Rust panics unwind across FFI; catch panics at every exported boundary and worker thread boundary.
   - Do not return Rust-owned `String` memory unless a matching free function is exported.
   - For streaming callbacks, pass `user_data` through unchanged and call callbacks with `(const char*, len)` so UE can copy by length.
   - Read [references/rust-bridge-template.md](references/rust-bridge-template.md) when writing or reviewing Rust code.

4. Generate a C-compatible header.
   - Use `cbindgen` and keep the ABI small: opaque handles, primitives, pointers, lengths, callbacks, status codes, and explicit free/cancel functions.
   - Prefer a C header with C++ compatibility guards.
   - Include this header in UE under `THIRD_PARTY_INCLUDES_START/END`.

5. Place artifacts predictably.
   - Headers: `ThirdParty/<RustLib>/include`.
   - Windows runtime DLLs: `ThirdParty/<RustLib>/bin/Win64`.
   - Windows import libraries: `ThirdParty/<RustLib>/lib/Win64`.
   - macOS dylibs: `ThirdParty/<RustLib>/lib/Mac`, and often copied to `Plugins/<Plugin>/Binaries/Mac` for editor runtime loading.
   - Do not rely on Cargo's `target/release` path at runtime.

6. Update `Build.cs`.
   - Always add the third-party include path.
   - `RuntimeDependencies.Add(...)` is staging metadata; it does not replace building/copying Rust artifacts.
   - For editor-time manual loading, copy dylibs/DLLs into `Plugins/<Plugin>/Binaries/<Platform>` or otherwise load from an explicit stable path.
   - For direct linked mode, add `PublicAdditionalLibraries` for the import library/dylib.
   - For Windows delay-load, add `PublicAdditionalLibraries`, `PublicDelayLoadDLLs`, copy all dependent DLLs to `Binaries/Win64`, and stage them with `RuntimeDependencies`.
   - For macOS manual loaded mode, do not hard-link the Rust bridge dylib. Copy dylibs to `Binaries/Mac`, stage them, then load with `FPlatformProcess::GetDllHandle`.
   - Read [references/unreal-integration-template.md](references/unreal-integration-template.md) when editing `Build.cs` or module startup code.

7. Wrap the FFI in the UE module.
   - For manual loaded mode, define typed function pointers in the module header and initialize them in `StartupModule`.
   - Load dependent libraries before the bridge library, then resolve exports from the bridge.
   - In `ShutdownModule`, null function pointers and free handles in a safe order.
   - Log missing library paths and missing exports clearly.
   - Never call `GetDllExport` and also directly call the same symbol on the same platform; that mixes manual and direct modes.

8. Build the Blueprint async wrapper.
   - Use `UBlueprintAsyncActionBase` for streaming or long-running work.
   - Start heavy FFI work on a background thread; never block the game thread with model load/generation/free if those can block.
   - Copy callback buffers immediately on the UE side before crossing threads.
   - Marshal all Blueprint delegate broadcasts to `ENamedThreads::GameThread`.
   - Guard callback ordering with `bFinished` so late `Done` after `Error`, or late callbacks after cancel, do not double-broadcast.
   - Keep the async action alive with `RegisterWithGameInstance`; use `AddToRoot` only when needed, and always release it on Done/Error/Cancel.
   - Cancel active streams when the owning PIE/world tears down, for example with `FWorldDelegates::OnWorldBeginTearDown`.
   - For AI-style token streams, read [references/streaming-callback-template.md](references/streaming-callback-template.md) and [references/blueprint-async-stream-template.md](references/blueprint-async-stream-template.md).

9. Verify the full path.
   - Run `cargo build --release` for each platform target.
   - Confirm output names: Windows MSVC usually produces `<crate>.dll` and `<crate>.dll.lib`; macOS produces `lib<crate>.dylib`.
   - Check exports with `nm`/`dumpbin`.
   - Check architectures with `file` or `lipo -info`.
   - Build the UE target from a clean editor/project build.
   - Test both PIE and packaged/staged output when runtime dependencies changed.

## Platform Notes

### Windows

- Build with `x86_64-pc-windows-msvc` for UE Win64.
- Link the import library, usually `<crate>.dll.lib`; if Cargo output differs, use the actual file name.
- Copy every runtime DLL dependency into `Plugins/<Plugin>/Binaries/Win64`.
- Use `PublicDelayLoadDLLs.Add("<crate>.dll")` for the bridge DLL.
- In `StartupModule`, preload dependent DLLs first, then the bridge DLL. Delay-load direct calls can then resolve through the loaded module.
- If direct calls fail at runtime, inspect dependency order and missing DLLs before changing architecture.

### macOS

- Build the same architecture as UE. On Apple Silicon this is usually `aarch64-apple-darwin`; Intel builds need `x86_64-apple-darwin`; universal plugins need `lipo`.
- For complex stacks such as LiteRT, TensorFlow Lite, Protobuf, Abseil, GPU delegates, or other large native runtimes, prefer manual loaded mode on Mac to avoid hard-link loader surprises.
- Copy `.dylib` files to `Plugins/<Plugin>/Binaries/Mac` for editor runtime loading, and stage them with `RuntimeDependencies`.
- Load dependency dylibs before the Rust bridge dylib.
- Resolve bridge exports with `FPlatformProcess::GetDllExport` and call through typed function pointers.
- Use `otool -L` to verify whether the UE module hard-links a dylib. In manual loaded mode, the UE module should not list the Rust bridge dylib as a load command.
- Use `otool -l` to inspect `LC_RPATH`; use `install_name_tool` when dylib install names or dependent paths do not resolve from the plugin folder.
- Ad-hoc sign patched dylibs when macOS code signing rejects modified binaries.

## Common Patterns

### Mac manual loaded bridge

Use this when direct hard-linking causes editor startup hangs or loader conflicts, but you still want in-process FFI:

```cpp
using FStartFunc = LiteRtLmEdgeStreamHandle* (*)(
    const char*, const char*, const char*, int32, void*,
    LiteRtLmEdgeTokenCallback,
    LiteRtLmEdgeErrorCallback,
    LiteRtLmEdgeDoneCallback);

void* BridgeHandle = FPlatformProcess::GetDllHandle(*BridgeDylibPath);
FStartFunc Start = reinterpret_cast<FStartFunc>(
    FPlatformProcess::GetDllExport(BridgeHandle, TEXT("litert_lm_edge_ue_stream_text_start")));
```

Keep the function pointer null-checked at call sites, and log a clear error if binding failed.

### Windows delay-load preload

Use this when direct calls are acceptable on Windows but DLL dependencies live in the plugin:

```csharp
PublicAdditionalLibraries.Add(BridgeImportLib);
PublicDelayLoadDLLs.Add("litert_lm_edge_ue_ffi.dll");
RuntimeDependencies.Add("$(TargetOutputDir)/litert_lm_edge_ue_ffi.dll", BridgeDll);
```

Then preload dependent DLLs in `StartupModule` with `FPlatformProcess::GetDllHandle`.

### Streaming callback wrapper

- Rust callback thread must not touch UObject/UI directly.
- UE callback should copy `(Data, Len)` into `std::string`/`FString`.
- Then use `AsyncTask(ENamedThreads::GameThread, ...)` before broadcasting delegates.
- First tokens from LLMs can be whitespace. Debug logs should include token length and sanitized token text.

## Diagnostics

- If the editor hangs during startup after adding `PublicAdditionalLibraries` on Mac, inspect `otool -L` for hard-linked Rust/ML dylibs and consider manual loaded mode.
- If runtime loading fails on Mac, log full dylib paths and inspect `dlerror()` where available.
- If Windows works in editor but packaged builds fail, inspect staged DLL locations and `RuntimeDependencies`.
- If Blueprint shows no output, verify logs for first token length; whitespace tokens may look blank in `PrintString`.
- If async callbacks continue after PIE stops, bind to world teardown and cancel/free the stream.
- If cancel/free blocks, do the free/join on a background thread.
- If there are no callbacks, verify that the async action is kept alive until Done/Error/Cancel.
- If a user requested no subprocess, search for `CreateProc`, request files, pipes, and worker executables before finalizing.

## Guardrails

- Keep the Rust ABI smaller than the Rust implementation. Expose stable C functions and hide Rust crates/types behind opaque handles.
- Prefer status codes plus out-pointers/callbacks for complex APIs.
- Never let Rust-owned memory cross into UE without an explicit free function or immediate callback-copy contract.
- Do not add a UE module as a dependency of itself.
- Do not do model load/generation/free on the game thread.
- Do not silently switch to a subprocess architecture. A worker process is a product/architecture decision, not an implementation detail.
- Do not leave unused worker binaries in the plugin if the selected mode is in-process FFI.
- Validate with platform tools (`otool`, `nm`, `dumpbin`, `file`, `lipo`) instead of relying only on build success.
