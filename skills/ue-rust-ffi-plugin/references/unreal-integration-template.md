# Unreal Integration Template

Use this when wiring a Rust `cdylib` into a UE5 C++ plugin/module.

## Suggested Plugin Layout

```text
Plugins/MyPlugin/
  MyPlugin.uplugin
  Source/
    MyPlugin/
      MyPlugin.Build.cs
      Public/
        MyRustBridge.h
      Private/
        MyRustBridge.cpp
  ThirdParty/
    UeMarkdownBridge/
      include/
        ue_markdown_bridge.h
      bin/
        Win64/
          ue_markdown_bridge.dll
      lib/
        Win64/
          ue_markdown_bridge.dll.lib
        Mac/
          libue_markdown_bridge.dylib
```

## Direct Linked Build.cs

Use this when C++ includes the generated header and calls Rust exports directly.

```csharp
using System.IO;
using UnrealBuildTool;

public class MyPlugin : ModuleRules
{
    public MyPlugin(ReadOnlyTargetRules Target) : base(Target)
    {
        PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;

        PublicDependencyModuleNames.AddRange(new[]
        {
            "Core",
            "Projects"
        });

        PrivateDependencyModuleNames.AddRange(new[]
        {
            "CoreUObject",
            "Engine"
        });

        string PluginRoot = Path.GetFullPath(Path.Combine(ModuleDirectory, "..", ".."));
        string ThirdParty = Path.Combine(PluginRoot, "ThirdParty", "UeMarkdownBridge");

        PublicSystemIncludePaths.Add(Path.Combine(ThirdParty, "include"));

        if (Target.Platform == UnrealTargetPlatform.Win64)
        {
            string DllName = "ue_markdown_bridge.dll";
            string DllPath = Path.Combine(ThirdParty, "bin", "Win64", DllName);
            string ImportLibPath = Path.Combine(ThirdParty, "lib", "Win64", "ue_markdown_bridge.dll.lib");

            PublicAdditionalLibraries.Add(ImportLibPath);
            PublicDelayLoadDLLs.Add(DllName);
            RuntimeDependencies.Add("$(TargetOutputDir)/" + DllName, DllPath);
        }
        else if (Target.Platform == UnrealTargetPlatform.Mac)
        {
            string DylibName = "libue_markdown_bridge.dylib";
            string DylibPath = Path.Combine(ThirdParty, "lib", "Mac", DylibName);

            PublicAdditionalLibraries.Add(DylibPath);
            RuntimeDependencies.Add("$(TargetOutputDir)/" + DylibName, DylibPath);
        }
    }
}
```

Do not add `"MyPlugin"` to its own dependency list.

## Direct Call C++ Wrapper

```cpp
// Public/MyRustBridge.h
#pragma once

#include "CoreMinimal.h"

class MYPLUGIN_API FMyRustBridge
{
public:
    static FString MarkdownFromUrl(const FString& Url);
};
```

```cpp
// Private/MyRustBridge.cpp
#include "MyRustBridge.h"

THIRD_PARTY_INCLUDES_START
#include "ue_markdown_bridge.h"
THIRD_PARTY_INCLUDES_END

FString FMyRustBridge::MarkdownFromUrl(const FString& Url)
{
    char* RawResult = ue_markdown_from_url(TCHAR_TO_UTF8(*Url));
    if (RawResult == nullptr)
    {
        return FString();
    }

    const FString Result = UTF8_TO_TCHAR(RawResult);
    ue_rust_free_string(RawResult);
    return Result;
}
```

Replace `MYPLUGIN_API` with the module API macro generated from the actual module name.

## Manual Runtime Loading Wrapper

Use this when you want to load from the plugin binary directory and resolve functions manually.

```cpp
// Public/MyRustBridge.h
#pragma once

#include "CoreMinimal.h"

class MYPLUGIN_API FMyRustBridge
{
public:
    static bool Load();
    static void Unload();
    static FString MarkdownFromUrl(const FString& Url);

private:
    using FMarkdownFromUrl = char* (*)(const char*);
    using FFreeString = void (*)(char*);

    static void* LibraryHandle;
    static FMarkdownFromUrl MarkdownFromUrlFn;
    static FFreeString FreeStringFn;
};
```

```cpp
// Private/MyRustBridge.cpp
#include "MyRustBridge.h"

#include "HAL/PlatformProcess.h"
#include "Interfaces/IPluginManager.h"
#include "Misc/Paths.h"

void* FMyRustBridge::LibraryHandle = nullptr;
FMyRustBridge::FMarkdownFromUrl FMyRustBridge::MarkdownFromUrlFn = nullptr;
FMyRustBridge::FFreeString FMyRustBridge::FreeStringFn = nullptr;

bool FMyRustBridge::Load()
{
    if (LibraryHandle != nullptr)
    {
        return true;
    }

    const TSharedPtr<IPlugin> Plugin = IPluginManager::Get().FindPlugin(TEXT("MyPlugin"));
    if (!Plugin.IsValid())
    {
        return false;
    }

#if PLATFORM_WINDOWS
    const FString LibraryPath = FPaths::Combine(Plugin->GetBaseDir(), TEXT("Binaries/Win64/ue_markdown_bridge.dll"));
#elif PLATFORM_MAC
    const FString LibraryPath = FPaths::Combine(Plugin->GetBaseDir(), TEXT("Binaries/Mac/libue_markdown_bridge.dylib"));
#else
    return false;
#endif

    LibraryHandle = FPlatformProcess::GetDllHandle(*LibraryPath);
    if (LibraryHandle == nullptr)
    {
        return false;
    }

    MarkdownFromUrlFn = reinterpret_cast<FMarkdownFromUrl>(
        FPlatformProcess::GetDllExport(LibraryHandle, TEXT("ue_markdown_from_url")));
    FreeStringFn = reinterpret_cast<FFreeString>(
        FPlatformProcess::GetDllExport(LibraryHandle, TEXT("ue_rust_free_string")));

    if (MarkdownFromUrlFn == nullptr || FreeStringFn == nullptr)
    {
        Unload();
        return false;
    }

    return true;
}

void FMyRustBridge::Unload()
{
    MarkdownFromUrlFn = nullptr;
    FreeStringFn = nullptr;

    if (LibraryHandle != nullptr)
    {
        FPlatformProcess::FreeDllHandle(LibraryHandle);
        LibraryHandle = nullptr;
    }
}

FString FMyRustBridge::MarkdownFromUrl(const FString& Url)
{
    if (!Load())
    {
        return FString();
    }

    char* RawResult = MarkdownFromUrlFn(TCHAR_TO_UTF8(*Url));
    if (RawResult == nullptr)
    {
        return FString();
    }

    const FString Result = UTF8_TO_TCHAR(RawResult);
    FreeStringFn(RawResult);
    return Result;
}
```

Call `FMyRustBridge::Load()` from module startup if the Rust library is required immediately. Call `Unload()` from `ShutdownModule`.

## Manual Loading Build.cs Notes

Manual loading does not require `PublicAdditionalLibraries` for Windows import libraries. It still needs include paths if you include generated typedefs or constants, and it still needs `RuntimeDependencies` or another copy step so the DLL/dylib exists at the path used by `GetDllHandle`.

For plugin-local manual loading, stage to plugin binaries:

```csharp
RuntimeDependencies.Add("$(PluginDir)/Binaries/Win64/ue_markdown_bridge.dll", DllPath);
RuntimeDependencies.Add("$(PluginDir)/Binaries/Mac/libue_markdown_bridge.dylib", DylibPath);
```

Also ensure the same files exist there during local editor development, since staging mainly affects build/package outputs.
