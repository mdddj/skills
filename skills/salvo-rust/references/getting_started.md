# Salvo-Rust - Getting Started

**Pages:** 1

---

## Quick Start

**URL:** llms-txt#quick-start

**Contents:**
- Install Rust
- Write Your First Salvo Program
- Detailed Explanation
- Sleek HTTP3
- Salvo CLI Tool 🛠️
  - Step 1
  - Step 2
- More Examples

If you haven't installed Rust yet, you can use the official (`rustup`)\[[https://doc.rust-lang.org/book/ch01-01-installation.html](https://doc.rust-lang.org/book/ch01-01-installation.html)] to install Rust.

:::tip
The minimum supported Rust version for Salvo is 1.89. Run `rustup update` to ensure you have a compatible Rust version installed.
:::

## Write Your First Salvo Program

Create a new project:

Add dependencies to `Cargo.toml`:

In `main.rs`, create a simple function handler named `hello`, which simply prints the text `"Hello world"`.

Congratulations! Your first Salvo program is complete. Just run `cargo run` in the command line, then open `http://127.0.0.1:8698` in your browser.

## Detailed Explanation

Here, `hello_world` is a `Handler` used to process user requests. The `#[handler]` attribute allows a function to conveniently implement the `Handler` trait. Moreover, it enables us to write function parameters in various simplified ways.

- You can omit unused parameters in the function. For example, `_req`, `_depot`, and `_ctrl` are not used here and can be omitted entirely:

- Any type can be used as the function's return type as long as it implements the `Writer` trait. For instance, `&str` implements `Writer`, and when returned, it prints plain text:

- More commonly, we need to use `Result<T, E>` as the return type to handle errors during function execution. If both `T` and `E` implement `Writer`, then `Result<T, E>` can be used as a return value:

They say HTTP3 is as agile as a swallow, a dream many programmers have longed for but couldn't achieve. This time, Salvo makes it happen, allowing everyone to effortlessly enjoy the wonderful services brought by HTTP3!

First, enable the HTTP3 feature in `Cargo.toml`, then modify `main.rs` as follows:

[Salvo CLI](https://github.com/salvo-rs/salvo-cli) is a tool designed for the Salvo web framework. It helps create clean, readable code, saving your time for more interesting things in life.

If you have ideas for improving the CLI or notice issues that need fixing, don't hesitate! Submit an issue—we welcome your insights.

Install the CLI tool:

> Create a new Salvo project using the `new` command followed by your project name:
>
>

With this simple CLI tool, you can quickly set up a Salvo project, allowing you to focus on implementing your business logic rather than project structure setup. ✨

It's recommended to clone the Salvo repository directly and run the examples in the `examples` directory. For instance, the following command runs the `hello` example:

There are many examples in the `examples` directory. You can run them using commands like `cargo run --bin example-<name>`.

---
url: /guide/concepts/index.md
---

---
url: /guide/concepts/router.md
---

**Examples:**

Example 1 (bash):
```bash
cargo new hello --bin
```

Example 2 (unknown):
```unknown
In `main.rs`, create a simple function handler named `hello`, which simply prints the text `"Hello world"`.
```

Example 3 (unknown):
```unknown
Congratulations! Your first Salvo program is complete. Just run `cargo run` in the command line, then open `http://127.0.0.1:8698` in your browser.

## Detailed Explanation

Here, `hello_world` is a `Handler` used to process user requests. The `#[handler]` attribute allows a function to conveniently implement the `Handler` trait. Moreover, it enables us to write function parameters in various simplified ways.

- Original form:
```

Example 4 (unknown):
```unknown
- You can omit unused parameters in the function. For example, `_req`, `_depot`, and `_ctrl` are not used here and can be omitted entirely:
```

---
