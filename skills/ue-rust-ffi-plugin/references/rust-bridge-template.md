# Rust Bridge Template

Use this when implementing the Rust side of a UE-callable dynamic library.

## Cargo.toml

```toml
[package]
name = "ue-markdown-bridge"
version = "0.1.0"
edition = "2021"

[lib]
name = "ue_markdown_bridge"
crate-type = ["cdylib"]

[dependencies]
mdka = "1.2"
reqwest = { version = "0.12", default-features = false, features = ["blocking", "rustls-tls"] }

[build-dependencies]
cbindgen = "0.27"
```

Use `cdylib`. Plain `dylib` exposes Rust-specific dynamic linking details and is not the right default for C/C++ consumers.

## cbindgen.toml

```toml
language = "C"
cpp_compat = true
include_guard = "UE_MARKDOWN_BRIDGE_H"
usize_is_size_t = true
```

## build.rs

```rust
use std::{env, fs, path::PathBuf};

fn main() {
    println!("cargo:rerun-if-changed=src/lib.rs");
    println!("cargo:rerun-if-changed=cbindgen.toml");

    let crate_dir = env::var("CARGO_MANIFEST_DIR").expect("CARGO_MANIFEST_DIR is set");
    let include_dir = PathBuf::from(&crate_dir).join("include");
    fs::create_dir_all(&include_dir).expect("create include directory");

    let config = cbindgen::Config::from_file("cbindgen.toml").unwrap_or_default();
    cbindgen::Builder::new()
        .with_crate(crate_dir)
        .with_config(config)
        .generate()
        .expect("generate C bindings")
        .write_to_file(include_dir.join("ue_markdown_bridge.h"));
}
```

## src/lib.rs

```rust
use std::ffi::{CStr, CString};
use std::os::raw::c_char;

fn into_ffi_string(value: String) -> *mut c_char {
    let sanitized = value.replace('\0', "");
    CString::new(sanitized)
        .unwrap_or_else(|_| CString::new("").expect("empty CString"))
        .into_raw()
}

fn error_string(message: impl AsRef<str>) -> *mut c_char {
    into_ffi_string(format!("ERROR: {}", message.as_ref()))
}

#[no_mangle]
pub extern "C" fn ue_markdown_from_url(url: *const c_char) -> *mut c_char {
    let result = std::panic::catch_unwind(|| {
        if url.is_null() {
            return Err("url pointer was null".to_owned());
        }

        let url = unsafe { CStr::from_ptr(url) }
            .to_str()
            .map_err(|err| format!("url was not valid UTF-8: {err}"))?;

        let html = reqwest::blocking::get(url)
            .and_then(|response| response.error_for_status())
            .and_then(|response| response.text())
            .map_err(|err| format!("request failed: {err}"))?;

        Ok(mdka::from_html(&html))
    });

    match result {
        Ok(Ok(markdown)) => into_ffi_string(markdown),
        Ok(Err(message)) => error_string(message),
        Err(_) => error_string("panic in Rust FFI boundary"),
    }
}

#[no_mangle]
pub extern "C" fn ue_rust_free_string(ptr: *mut c_char) {
    if ptr.is_null() {
        return;
    }

    unsafe {
        drop(CString::from_raw(ptr));
    }
}
```

For Rust 2024 edition, use `#[unsafe(no_mangle)]` instead of `#[no_mangle]`.

## Build Commands

macOS native:

```bash
cargo build --release
```

macOS universal dylib:

```bash
rustup target add aarch64-apple-darwin x86_64-apple-darwin
cargo build --release --target aarch64-apple-darwin
cargo build --release --target x86_64-apple-darwin
lipo -create \
  -output target/release/libue_markdown_bridge.dylib \
  target/aarch64-apple-darwin/release/libue_markdown_bridge.dylib \
  target/x86_64-apple-darwin/release/libue_markdown_bridge.dylib
```

Windows Win64 on a Windows/MSVC build host:

```powershell
rustup target add x86_64-pc-windows-msvc
cargo build --release --target x86_64-pc-windows-msvc
```

Expected MSVC outputs:

```text
target/x86_64-pc-windows-msvc/release/ue_markdown_bridge.dll
target/x86_64-pc-windows-msvc/release/ue_markdown_bridge.dll.lib
```

If Cargo emits a different import library name, use the emitted file name in UE `Build.cs`.
