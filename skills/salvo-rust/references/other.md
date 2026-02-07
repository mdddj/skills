# Salvo-Rust - Other

**Pages:** 33

---

## CSRF Protection

**URL:** llms-txt#csrf-protection

**Contents:**
- What is CSRF?
- How CSRF Attacks Work
- Protection Strategies
- CSRF Implementation in Salvo

CSRF (Cross-Site Request Forgery) is a web security vulnerability where attackers trick authenticated users into performing unintended actions without their knowledge. In simple terms, attackers exploit a user's logged-in status to send malicious requests on their behalf.

## How CSRF Attacks Work

The typical attack process involves:

1. The user logs into the target website A and obtains an authentication cookie.
2. The user visits a malicious website B.
3. Code on malicious website B automatically sends a request to website A, carrying the user's cookie.
4. Website A cannot distinguish whether this is a malicious request or a legitimate user action.

## Protection Strategies

Salvo provides CSRF middleware to protect your applications from such attacks:

- Add a hidden CSRF token to forms.
- Verify that user-submitted requests contain a valid CSRF token.
- By default, validate POST, PATCH, DELETE, and PUT requests.

## CSRF Implementation in Salvo

`Csrf` is a struct that implements the `Handler` trait and includes an internal `skipper` field to specify requests that should skip validation. By default, it validates `Method::POST`, `Method::PATCH`, `Method::DELETE`, and `Method::PUT` requests.

Salvo supports two CSRF token storage methods:

1. **CookieStore**: Stores the token in a cookie and verifies whether the `csrf-token` in the request headers or form matches the cookie value.
2. **SessionStore**: Stores the token in the session and requires the use of session middleware.

_**Example Code**_ (cookie store)

_**Example Code**_ (session store)

---
url: /guide/features/flash.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

Example 2 (unknown):
```unknown
_**Example Code**_ (session store)


**main.rs**
```

Example 3 (unknown):
```unknown
**Cargo.toml**
```

---

## Writing Tests

**URL:** llms-txt#writing-tests

**Contents:**
- The Importance of Testing
- Salvo Testing Tools

## The Importance of Testing

Writing tests is a hallmark of responsible engineering and the secret to sleeping soundly. A comprehensive test suite not only improves code quality and prevents regression errors but also ensures you rest easy after deployment. While your colleagues might be jolted awake by late-night system crash alerts, your application remains rock-solid—this is the tranquility and confidence that testing brings.

## Salvo Testing Tools

The test module provided by Salvo helps you test Salvo-based projects.

[Latest Documentation](https://docs.rs/salvo_core/latest/salvo_core/test/index.html)

---
url: /guide/topics/deployment.md
---

**Examples:**

Example 1 (rust):
```rust
use salvo::prelude::*;

#[handler]
async fn hello_world() -> &'static str {
    "Hello World"
}

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();

    tracing::info!("Listening on http://127.0.0.1:8698");
    let acceptor = TcpListener::new("127.0.0.1:8698").bind().await; Server::new(acceptor).serve(route()).await;
}

fn route() -> Router {
    Router::new().get(hello_world)
}

#[cfg(test)]
mod tests {
    use salvo::prelude::*;
    use salvo::test::{ResponseExt, TestClient};

    #[tokio::test]
    async fn test_hello_world() {
        let service = Service::new(super::route());

        let content = TestClient::get(format!("http://127.0.0.1:8698/"))
            .send(&service)
            .await
            .take_string()
            .await
            .unwrap();
        assert_eq!(content, "Hello World");
    }
}
```

---

## To Master This Art

**URL:** llms-txt#to-master-this-art

**Contents:**
- Why Build This Framework
- Is Salvo Right for You?
- Achieving Simplicity
- Routing System

## Why Build This Framework

As a beginner, I found myself struggling to grasp existing frameworks like actix-web and Rocket. When I attempted to rewrite a previous Go web service in Rust, each framework seemed more complex than those available in Go. Given Rust's already steep learning curve, why complicate web frameworks further?

When Tokio introduced the Axum framework, I was thrilled, thinking I could finally stop maintaining my own web framework. However, I soon realized that while Axum appeared simple, it required extensive type gymnastics and generic definitions. Crafting even a basic middleware demanded deep Rust expertise and writing copious amounts of obscure boilerplate code.

Thus, I decided to continue maintaining my unique web framework—one that is intuitive, feature-rich, and beginner-friendly.

## Is Salvo Right for You?

Salvo is simple yet comprehensive and powerful, arguably the strongest in the Rust ecosystem. Despite its capabilities, it remains easy to learn and use, sparing you any "self-castration" pains.

- **Ideal for Rust beginners**: CRUD operations are commonplace, and with Salvo, such tasks feel as straightforward as with frameworks in other languages (e.g., Express, Koa, Gin, Flask). In some aspects, Salvo is even more abstract and concise.

- **Suitable for production**: If you aim to deploy Rust in production for robust, high-speed servers, Salvo fits the bill. Although not yet at version 1.0, its core features have undergone years of iteration, ensuring stability and timely issue resolution.

- **Perfect for you**, especially if your hair is thinning and shedding daily.

## Achieving Simplicity

Hyper handles many low-level implementations, making it a reliable foundation for general needs. Salvo follows this approach, offering a powerful and flexible routing system alongside essential features like Acme, OpenAPI, and JWT authentication.

Salvo unifies Handlers and Middleware: Middleware _is_ a Handler. Both are attached to the Router via `hoop`. Essentially, they process `Request` and may write data to `Response`. A Handler receives three parameters: `Request`, `Depot` (for temporary data during request processing), and `Response`.

For convenience, you can omit unnecessary parameters or ignore their order.

The routing API is exceptionally simple yet powerful. For typical use cases, you only need to focus on the `Router` type.

Additionally, if a struct implements relevant traits, Salvo can automatically generate OpenAPI documentation, extract parameters, handle errors gracefully, and return user-friendly messages. This makes writing handlers as intuitive as writing ordinary functions. We'll explore these features in detail later. Here’s an example:

In this example, `JsonBody<CreateOrUpdateMessageLog>` automatically parses JSON from the request body into the `CreateOrUpdateMessageLog` type (supporting multiple data sources and nested types). The `#[endpoint]` macro generates OpenAPI documentation for this endpoint, simplifying parameter extraction and error handling.

I believe Salvo’s routing system stands out. Routers can be flat or tree-like, distinguishing between _business logic trees_ and _access directory trees_. The business logic tree organizes routers based on business needs, which may not align with the access directory tree.

Typically, routes are written like this:

Often, viewing articles and listing them don’t require user login, but creating, editing, or deleting articles does. Salvo’s nested routing system elegantly handles this. We can group public routes together:

Then, group protected routes with middleware for authentication:

Even though both routers share the same `path("articles")`, they can be added to the same parent router, resulting in:

`{id}` matches a path segment. Typically, an article `id` is numeric, so we can restrict matching with a regex: `r"{id:/\d+/}"`.

---
url: /guide/quick-start.md
---

**Examples:**

Example 1 (rust):
```rust
use salvo::prelude::*;

#[handler]
async fn hello_world(_req: &mut Request, _depot: &mut Depot, res: &mut Response) {
    res.render("Hello world");
}
#[handler]
async fn hello_world(res: &mut Response) {
    res.render("Hello world");
}
```

Example 2 (rust):
```rust
#[endpoint(tags("message_logs"))]
pub async fn create_message_log_handler(
    input: JsonBody<CreateOrUpdateMessageLog>,
    depot: &mut Depot,
) -> APPResult<Json<MessageLog>> {
    let db = utils::get_db(depot)?;
    let log = create_message_log(&input, db).await?;
    Ok(Json(log))
}
```

Example 3 (rust):
```rust
Router::new().path("articles").get(list_articles).post(create_article);
Router::new()
    .path("articles/{id}")
    .get(show_article)
    .patch(edit_article)
    .delete(delete_article);
```

Example 4 (rust):
```rust
Router::new()
    .path("articles")
    .get(list_articles)
    .push(Router::new().path("{id}").get(show_article));
```

---

## HTTP/3 Support

**URL:** llms-txt#http/3-support

**Contents:**
- Enabling HTTP/3 Support
- HTTP/3 Use Cases
- Example Code
- Key Code Analysis
  - TLS Configuration
  - Combining Listeners
- Running the Example
- Considerations

Salvo provides support for HTTP/3, which can be enabled via the `quinn` feature. HTTP/3 is based on the QUIC protocol and offers lower latency and better performance compared to traditional HTTP/1.1 and HTTP/2, especially in unstable network environments.

## Enabling HTTP/3 Support

To enable HTTP/3 support in Salvo, you need to activate the `quinn` feature in your `Cargo.toml` file:

HTTP/3 is particularly suitable for the following scenarios:

- Applications on mobile devices and in unstable network environments
- Real-time applications requiring low latency
- Scenarios involving parallel downloads of numerous small files
- Applications needing connection migration (e.g., seamless switching from WiFi to cellular networks without connection interruption)

The following is a simple HTTP/3 server example that supports both HTTP/3 (QUIC) and HTTPS (TCP):

### TLS Configuration

Since HTTP/3 is based on the QUIC protocol, which requires TLS 1.3 for encryption, TLS certificates and keys must be configured. In Salvo, we use `RustlsConfig` to configure TLS.

### Combining Listeners

This code is the core part of handling HTTP/3 in Salvo. It first creates a TLS-enabled TCP listener (for HTTP/1.1 and HTTP/2), then creates a QUIC listener (for HTTP/3). The `join` method combines these two listeners, enabling the server to handle requests from different protocols simultaneously.

## Running the Example

To run this example, you need valid TLS certificates and private keys. In a development environment, self-signed certificates can be used. The complete example code can be found in the [Salvo GitHub repository](https://github.com/salvo-rs/salvo/tree/main/examples/hello-h3).

Note that since many clients do not yet fully support HTTP/3, it is essential for this server to support both HTTP/3 and HTTPS.

1. HTTP/3 requires TLS 1.3 support, so valid certificates and keys must be configured.
2. Clients must support the HTTP/3 protocol to utilize this feature; otherwise, they will fall back to HTTP/1.1 or HTTP/2.
3. In production environments, certificates issued by a trusted Certificate Authority (CA) should be used instead of self-signed certificates.

---
url: /guide/features/affix-state.md
---

**Examples:**

Example 1 (toml):
```toml
salvo = { workspace = true, features = ["quinn"] }
```

Example 2 (rust):
```rust
use salvo::conn::rustls::{Keycert, RustlsConfig};
use salvo::prelude::*;

// Handler function that responds with "Hello World"
#[handler]
async fn hello() -> &'static str {
    "Hello World"
}

#[tokio::main]
async fn main() {
    // Initialize the logging system
    tracing_subscriber::fmt().init();

    // Load TLS certificate and private key from embedded PEM files
    let cert = include_bytes!("../certs/cert.pem").to_vec();
    let key = include_bytes!("../certs/key.pem").to_vec();

    // Create a router and add an endpoint
    let router = Router::new().get(hello);

    // Configure TLS settings using Rustls
    let config = RustlsConfig::new(Keycert::new().cert(cert.as_slice()).key(key.as_slice()));

    // Create a TLS-encrypted TCP listener on port 8698
    let listener = TcpListener::new(("0.0.0.0", 8698)).rustls(config.clone());

    // Create a QUIC listener and combine it with the TCP listener
    let acceptor = QuinnListener::new(config.build_quinn_config().unwrap(), ("0.0.0.0", 8698))
        .join(listener)
        .bind()
        .await;

    // Start the server supporting both HTTP/3 (QUIC) and HTTPS (TCP)
    Server::new(acceptor).serve(router).await;
}
```

Example 3 (rust):
```rust
// Configure TLS settings using Rustls
let config = RustlsConfig::new(Keycert::new().cert(cert.as_slice()).key(key.as_slice()));
```

Example 4 (rust):
```rust
// Create a TLS-encrypted TCP listener
let listener = TcpListener::new(("0.0.0.0", 8698)).rustls(config.clone());

// Create a QUIC listener and combine it with the TCP listener
let acceptor = QuinnListener::new(config.build_quinn_config().unwrap(), ("0.0.0.0", 8698))
    .join(listener)
    .bind()
    .await;
```

---

## Server-Sent Events (SSE)

**URL:** llms-txt#server-sent-events-(sse)

**Contents:**
- What is SSE
- Chat Application Example
- Integration with Large Language Models

Server-Sent Events (SSE) is a web technology that allows servers to push data to clients. Unlike WebSocket, SSE is a one-way communication method, where data can only be sent from the server to the client. It is suitable for scenarios where the server needs to push data in real-time, but the client does not need to send data frequently. SSE is based on the HTTP protocol, easy to implement, and comes with built-in reconnection mechanisms.

Provides middleware support for `SSE`.

## Chat Application Example

## Integration with Large Language Models

When integrating AI using OpenAI SDK or other large language model SDKs, streaming responses (Stream) are a common requirement. SSE provides a perfect solution, enabling real-time delivery of AI-generated content to the frontend, thereby offering a better user experience.

For example, when using OpenAI's Chat Completion API with streaming responses, Salvo's SSE functionality can be leveraged to send AI-generated content incrementally to the client, eliminating the delay of waiting for a complete response.

---
url: /guide/features/third-party.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

Example 2 (unknown):
```unknown
## Chat Application Example


**main.rs**
```

Example 3 (unknown):
```unknown
**Cargo.toml**
```

---

## Copy actual source code and build the application

**URL:** llms-txt#copy-actual-source-code-and-build-the-application

COPY src ./src/
RUN touch src/main.rs && \
    cargo build --release

---

## use salvo_oapi::schema::Object;

**URL:** llms-txt#use-salvo_oapi::schema::object;

**Contents:**
  - Error Handling

fn custom_type() -> Object {
    Object::new()
        .schema_type(salvo_oapi::SchemaType::String)
        .format(salvo_oapi::SchemaFormat::Custom(
            "email".to_string(),
        ))
        .description("this is the description")
}

#[derive(salvo_oapi::ToParameters, serde::Deserialize)]
#[salvo(parameters(default_parameter_in = Query))]
struct Query {
    #[salvo(parameter(schema_with = custom_type))]
    email: String,
}
rust
use salvo::http::{StatusCode, StatusError};
use salvo::oapi::{self, EndpointOutRegister, ToSchema};

impl EndpointOutRegister for Error {
    fn register(components: &mut oapi::Components, operation: &mut oapi::Operation) {
        operation.responses.insert(
            StatusCode::INTERNAL_SERVER_ERROR.as_str(),
            oapi::Response::new("Internal server error").add_content("application/json", StatusError::to_schema(components)),
        );
        operation.responses.insert(
            StatusCode::NOT_FOUND.as_str(),
            oapi::Response::new("Not found").add_content("application/json", StatusError::to_schema(components)),
        );
        operation.responses.insert(
            StatusCode::BAD_REQUEST.as_str(),
            oapi::Response::new("Bad request").add_content("application/json", StatusError::to_schema(components)),
        );
    }
}
rust
#[endpoint(status_codes(201, 409))]
pub async fn create_todo(new_todo: JsonBody<Todo>) -> Result<StatusCode, Error> {
    Ok(StatusCode::CREATED)
}
```

[to_schema]: https://docs.rs/salvo-oapi/latest/salvo_oapi/trait.ToSchema.html

[known_format]: https://docs.rs/salvo-oapi/latest/salvo_oapi/enum.KnownFormat.html

{/* [xml]: openapi/xml/struct.Xml.html */}

[to_parameters]: https://docs.rs/salvo-oapi/latest/salvo_oapi/trait.ToParameters.html

[path_parameters]: /guide/features/openapi.md#parameters-attributes

[struct]: https://doc.rust-lang.org/std/keyword.struct.html

[style]: https://docs.rs/salvo-oapi/latest/salvo_oapi/enum.ParameterStyle.html

[in_enum]: https://docs.rs/salvo-oapi/latest/salvo_oapi/enum.ParameterIn.html

[primitive]: https://doc.rust-lang.org/std/primitive/index.html

[serde attributes]: https://serde.rs/attributes.html

[std_string]: https://doc.rust-lang.org/std/string/struct.String.html

---
url: /guide/features/proxy.md
---

**Examples:**

Example 1 (unknown):
```unknown
- `rename_all = ...`: Supports a syntax similar to `serde` to define rules for renaming fields. If both `#[serde(rename_all = "...")]` and `#[salvo(schema(rename_all = "..."))]` are defined, `#[serde(rename_all = "...")]` is preferred.

- `symbol = ...`: A string literal used to define the name path of the structure on the line in OpenAPI. For example, `#[salvo(schema(symbol = "path.to.Pet"))]`.

- `default`: You can use the `Default` implementation of the structure to fill all fields with default values.

### Error Handling

For general applications, we will define a global error type (AppError) and implement `Writer` or `Scribe` for `AppError` so that errors can be sent to the client as web page information.

For OpenAPI, in order to get the necessary error information, we also need to implement `EndpointOutRegister` for this error:
```

Example 2 (unknown):
```unknown
This error set defines all possible error messages that the entire web application may return. However, in many cases, our `Handler` may only contain a few specific error types. In this case, you can use `status_codes` to filter out the required error type information:
```

---

## Flash

**URL:** llms-txt#flash

**Contents:**
  - Cookie Storage Example
  - Session Storage Example

A middleware that provides Flash Message functionality.

`FlashStore` offers data storage and retrieval operations. `CookieStore` stores data in `Cookies`, while `SessionStore` stores data in `Session`. `SessionStore` must be used in conjunction with the `session` feature.

### Cookie Storage Example

### Session Storage Example

---
url: /guide/features/force-https.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

Example 2 (unknown):
```unknown
### Session Storage Example


**main.rs**
```

Example 3 (unknown):
```unknown
**Cargo.toml**
```

---

## Copy dependency files first to build dependencies (leveraging cache layers)

**URL:** llms-txt#copy-dependency-files-first-to-build-dependencies-(leveraging-cache-layers)

COPY Cargo.toml Cargo.lock ./
RUN mkdir src && \
    echo 'fn main() { println!("Placeholder"); }' > src/main.rs && \
    cargo build --release

---

## Jemallocator: Rust jemalloc Memory Allocator Alternative

**URL:** llms-txt#jemallocator:-rust-jemalloc-memory-allocator-alternative

**Contents:**
- jemalloc Ecosystem
- Usage
  - Adding Dependencies
  - Setting as Global Allocator
- Advantages
- Compatibility Notes

:::tip
The default allocator may sometimes fail to release memory promptly. It is recommended to use [jemallocator](https://docs.rs/tikv-jemallocator/latest/tikv_jemallocator/) as a global allocator to replace the default one.
:::

[jemallocator](https://docs.rs/tikv-jemallocator/latest/tikv_jemallocator/) is a library linked with the jemalloc memory allocator, providing the `Jemalloc` unit type that implements the allocator API and can be set as `#[global_allocator]`.

`tikv-jemallocator` is the successor project to `jemallocator`. These two crates are identical except for their names. For new projects, it is recommended to use the `tikv-xxx` version.

## jemalloc Ecosystem

The jemalloc support ecosystem consists of the following crates:

- **tikv-jemalloc-sys**: Builds and links to jemalloc, exposing raw C bindings to it.
- **tikv-jemallocator**: Provides the `Jemalloc` type that implements the `GlobalAlloc` and `Alloc` traits.
- **tikv-jemalloc-ctl**: High-level wrappers for jemalloc control and introspection APIs (the `mallctl*()` function family and `MALLCTL NAMESPACE`).

### Adding Dependencies

To use tikv-jemallocator, add it as a dependency:

### Setting as Global Allocator

To set `tikv_jemallocator::Jemalloc` as the global allocator, add the following code to your project:

That's it! Once this static variable is defined, jemalloc will be used for all memory allocations requested by Rust code within the same program.

jemalloc is a general-purpose malloc implementation focusing on:

- Reducing memory fragmentation
- Scalability in high-concurrency scenarios
- Providing rich introspection and control capabilities

It is particularly useful in the following scenarios:

- Long-running applications
- Memory-intensive workloads
- High-performance services requiring fine-grained memory management

## Compatibility Notes

jemalloc does not support the MSVC target environment and is therefore unavailable when using the MSVC toolchain on Windows. This is why the `cfg(not(target_env = "msvc"))` condition is included in the example code.

---
url: /guide/ecology/reqwest.md
---

**Examples:**

Example 1 (toml):
```toml
[dependencies]

[target.'cfg(not(target_env = "msvc"))'.dependencies]
tikv-jemallocator = "0.5"
```

Example 2 (rust):
```rust
#[cfg(not(target_env = "msvc"))]
use tikv_jemallocator::Jemalloc;

#[cfg(not(target_env = "msvc"))]
#[global_allocator]
static GLOBAL: Jemalloc = Jemalloc;
```

---

## Chrono: Rust Date and Time Library

**URL:** llms-txt#chrono:-rust-date-and-time-library

**Contents:**
- Features
  - Default Features:
  - Optional Features:
- Overview
  - Time Difference/Duration
  - Date and Time
  - Formatting and Parsing
  - Conversion to/from Epoch Timestamps
- Limitations

[Chrono](https://docs.rs/chrono/latest/chrono/) aims to provide all functionality needed to do correct operations on dates and times in the proleptic Gregorian calendar:

- The `DateTime` type is timezone-aware by default, with separate timezone-naive types.
- Operations that may produce an invalid or ambiguous date and time return `Option` or `MappedLocalTime`.
- Configurable parsing and formatting with a strftime-inspired date and time formatting syntax.
- The `Local` timezone works with the current local timezone of the OS.
- Types and operations are implemented to be reasonably efficient.
- To avoid increasing the binary size, Chrono does not ship with timezone data by default. Use the companion crates `Chrono-TZ` or `tzfile` for full timezone support.

[Chrono](https://docs.rs/chrono/latest/chrono/) supports various runtime environments and operating systems, with several features that can be enabled or disabled.

### Default Features:

- `alloc`: Enables features that depend on allocation (primarily string formatting).
- `std`: Enables features that depend on the standard library. This is a superset of `alloc`, adding interoperation with standard library types and traits.
- `clock`: Enables reading the local timezone (`Local`). This is a superset of `now`.
- `now`: Enables reading the system time (`now`).
- `wasmbind`: Provides an interface to the JS Date API for the wasm32 target.

### Optional Features:

- `serde`: Enables serialization/deserialization via serde.
- `rkyv`: Deprecated, use the `rkyv-*` features.
- `rkyv-16`, `rkyv-32`, `rkyv-64`: Enables serialization/deserialization via rkyv, using 16-bit, 32-bit, or 64-bit integers respectively.
- `rkyv-validation`: Enables rkyv validation support using bytecheck.
- `arbitrary`: Constructs arbitrary instances of types with the Arbitrary crate.
- `unstable-locales`: Enables localization. This adds various methods with the `_localized` suffix.

### Time Difference/Duration

Chrono provides the `TimeDelta` type to represent the magnitude of a time span. This is an "exact" duration represented in seconds and nanoseconds, and does not represent "nominal" components such as days or months.

The `TimeDelta` type was previously named `Duration` (still available as a type alias). A notable difference from the similar `core::time::Duration` is that it is a signed value rather than unsigned.

Chrono provides the `DateTime` type to represent a date and time in a timezone.

`DateTime` is timezone-aware and must be constructed from a `TimeZone` object, which defines how the local date is converted to and from UTC. There are three well-known `TimeZone` implementations:

- `Utc` specifies the UTC timezone. It is most efficient.
- `Local` specifies the system local timezone.
- `FixedOffset` specifies an arbitrary fixed offset timezone, such as UTC+09:00 or UTC-10:30.

`DateTime` values with different `TimeZone` types are distinct and cannot be mixed, but can be converted to each other using the `DateTime::with_timezone` method.

You can get the current date and time in the UTC timezone (`Utc::now()`) or in the local timezone (`Local::now()`).

Additionally, you can create your own date and time:

### Formatting and Parsing

Formatting is done via the `format` method, which format is equivalent to the familiar strftime format.

The default `to_string` method and the `{:?}` specifier also give a reasonable representation. Chrono also provides `to_rfc2822` and `to_rfc3339` methods for common formats.

Chrono now also provides date formatting in almost any language without additional C libraries. This feature is available under the `unstable-locales` feature:

Parsing can be done in two ways:

1. The standard `FromStr` trait (and the `parse` method on strings) can be used to parse `DateTime<FixedOffset>`, `DateTime<Utc>`, and `DateTime<Local>` values.
2. `DateTime::parse_from_str` parses a date and time with an offset and returns `DateTime<FixedOffset>`.

### Conversion to/from Epoch Timestamps

Construct `DateTime<Utc>` from a UNIX timestamp using `DateTime::from_timestamp(seconds, nanoseconds)`.

Get the timestamp (in seconds) from a `DateTime` using `DateTime.timestamp`. Additionally, you can get the additional number of nanoseconds with `DateTime.timestamp_subsec_nanos`.

- Only the proleptic Gregorian calendar is supported (i.e., extended to support dates before the era).
- Date types are limited to approximately +/- 262,000 years from the common era.
- Time types are limited to nanosecond precision.
- Leap seconds can be represented, but Chrono does not fully support them.

---
url: /guide/ecology/jemallocator.md
---

**Examples:**

Example 1 (rust):
```rust
use chrono::prelude::*;

let utc: DateTime<Utc> = Utc::now(); // e.g., `2014-11-28T12:45:59.324310806Z`
let local: DateTime<Local> = Local::now(); // e.g., `2014-11-28T21:45:59.324310806+09:00`
```

Example 2 (rust):
```rust
use chrono::offset::MappedLocalTime;
use chrono::prelude::*;

let dt = Utc.with_ymd_and_hms(2014, 7, 8, 9, 10, 11).unwrap(); // `2014-07-08T09:10:11Z`
```

Example 3 (rust):
```rust
use chrono::prelude::*;

let dt = Utc.with_ymd_and_hms(2014, 11, 28, 12, 0, 9).unwrap();
assert_eq!(dt.format("%Y-%m-%d %H:%M:%S").to_string(), "2014-11-28 12:00:09");
assert_eq!(dt.format_localized("%A %e %B %Y, %T", Locale::fr_BE).to_string(), 
           "vendredi 28 novembre 2014, 12:00:09");
```

Example 4 (rust):
```rust
use chrono::prelude::*;

let dt = "2014-11-28T12:00:09Z".parse::<DateTime<Utc>>().unwrap();
let fixed_dt = DateTime::parse_from_str("2014-11-28 21:00:09 +09:00", "%Y-%m-%d %H:%M:%S %z").unwrap();
```

---

## Rate Limiting

**URL:** llms-txt#rate-limiting

**Contents:**
- Key Features
  - Static Quota Example
  - Dynamic Quota Example

Middleware providing rate limiting functionality.

- `RateIssuer` provides an abstraction for identifying visitor keys. `RemoteIpIssuer` is one of its implementations, which determines visitors based on their IP addresses. Keys are not necessarily strings; any type satisfying the `Hash + Eq + Send + Sync + 'static` constraints can serve as a key.

- `RateGuard` offers an abstraction for rate limiting algorithms. Default implementations include fixed window (`FixedGuard`) and sliding window (`SlidingGuard`).

- `RateStore` provides data storage operations. `MokaStore` is a built-in in-memory cache implementation based on `moka`. You can also define your own implementation.

- `RateLimiter` is a struct implementing `Handler`, which includes a `skipper` field to specify requests that should bypass rate limiting. By default, `none_skipper` is used, meaning no requests are skipped.

- `QuotaGetter` provides an abstraction for retrieving quotas. It fetches a quota object based on the visitor's `Key`, allowing user quotas and other configurations to be stored in databases and dynamically modified or retrieved.

### Static Quota Example

### Dynamic Quota Example

---
url: /guide/features/request-id.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

Example 2 (unknown):
```unknown
### Dynamic Quota Example


**main.rs**
```

Example 3 (unknown):
```unknown
**Cargo.toml**
```

---

## Sponsorship Project

**URL:** llms-txt#sponsorship-project

Our meeting here is a stroke of fate. In the vast expanse of the internet, the probability of our encounter is even smaller than the chance of us being born into this world. How about supporting the project a little? Let me remember you, and let your generosity spur me forward when I feel lazy, encouraging me to keep updating the project and making it better and better.

You can transfer funds directly via Alipay or WeChat, or [**Buy Me a Coffee ☕**](https://ko-fi.com/chrislearn):

| Alipay                        | WeChat                        |
| ----------------------------- | ----------------------------- |
| ![Alipay](/images/alipay.png) | ![WeChat](/images/weixin.png) |

---
url: /index.md
---

---

## Using Template Engines

**URL:** llms-txt#using-template-engines

Salvo does not come with any built-in template engine, as preferences for template engine styles vary from person to person.

At its core, a template engine is simply: data + template = string.

Therefore, as long as the final string can be rendered, any template engine can be supported.

For example, support for `askama`:

**templates/hello.html**

**Note**: For projects that are not particularly complex, we highly recommend adopting a frontend-backend separation approach. Use more flexible and ecosystem-rich UI frameworks (such as React, Vue, Svelte, etc.) to build the frontend, with Salvo serving as the backend API service. This approach leads to higher development efficiency, clearer responsibilities between frontend and backend, and better aligns with modern web application development trends.

---
url: /guide/topics/graceful-shutdown.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

Example 2 (unknown):
```unknown
**templates/hello.html**
```

---

## Using Databases

**URL:** llms-txt#using-databases

**Contents:**
  - [Diesel](https://diesel.rs/)
  - [Sqlx](https://github.com/launchbadge/sqlx)
  - [rbatis](https://github.com/rbatis/rbatis)
- [SeaORM](https://www.sea-ql.org/SeaORM/)
- [Tokio ORM (Toasty)](https://github.com/tokio-rs/toasty)
- [SurrealDB Rust SDK](https://surrealdb.com/docs/sdk/rust)

### [Diesel](https://diesel.rs/)

### [Sqlx](https://github.com/launchbadge/sqlx)

### [rbatis](https://github.com/rbatis/rbatis)

## [SeaORM](https://www.sea-ql.org/SeaORM/)

SeaORM is an asynchronous, dynamic ORM that provides robust relational database support, including entity relationships, a migration system, and a comprehensive type-safe query builder. It is well-suited for medium to large-scale projects requiring a full-featured ORM.

## [Tokio ORM (Toasty)](https://github.com/tokio-rs/toasty)

Toasty is an ORM developed by the Tokio team, currently under active development. It focuses on providing a tightly integrated ORM solution for the Tokio ecosystem. It may be suitable for projects that rely on Tokio and are open to adopting emerging technologies.

## [SurrealDB Rust SDK](https://surrealdb.com/docs/sdk/rust)

The SurrealDB Rust SDK provides connectivity to this multi-model database, making it suitable for applications that need to handle graph data, document data, and relational data. It is an excellent choice for projects requiring flexible data models.

---
url: /guide/topics/use-template-engine.md
---

**Examples:**

Example 1 (rust):
```rust
use diesel::prelude::*;
use diesel::r2d2::{ConnectionManager, Pool, PoolError, PooledConnection};
use once_cell::sync::OnceCell;
use salvo::prelude::*;

const DB_URL: &str = "postgres://benchmarkdbuser:benchmarkdbpass@tfb-database/hello_world";
type PgPool = Pool<ConnectionManager<PgConnection>>;

static DB_POOL: OnceCell<PgPool> = OnceCell::new();

fn connect() -> Result<PooledConnection<ConnectionManager<PgConnection>>, PoolError> {
    DB_POOL.get().unwrap().get()
}
fn build_pool(database_url: &str, size: u32) -> Result<PgPool, PoolError> {
    let manager = ConnectionManager::<PgConnection>::new(database_url);
    diesel::r2d2::Pool::builder()
        .max_size(size)
        .min_idle(Some(size))
        .test_on_check_out(false)
        .idle_timeout(None)
        .max_lifetime(None)
        .build(manager)
}

fn main() {
    DB_POOL
        .set(build_pool(&DB_URL, 10).expect(&format!("Error connecting to {}", &DB_URL)))
        .ok();
}

#[handler]
async fn show_article(req: &mut Request, res: &mut Response) -> Result<(), Error> {
    let id: i64 = req.param::<i64>("id").unwrap_or_default();
    let conn = connect()?;
    let article = articles::table.find(id).first::<Article>(&conn)?;
    res.render(Json(row));
    Ok(())
}
```

Example 2 (rust):
```rust
use sqlx::{Pool, PgPool};
use once_cell::sync::OnceCell;

pub static DB_POOL: OnceCell<PgPool> = OnceCell::new();

pub fn db_pool() -> &PgPool {
    DB_POOL.get().unwrap()
}

pub async fn make_db_pool(db_url: &str) -> PgPool {
    Pool::connect(&db_url).await.unwrap()
}

#[tokio::main]
async fn main() {
    let pool = make_db_pool().await;
    DB_POOL.set(pool).unwrap();
}
```

Example 3 (toml):
```toml
[dependencies]
async-std = "1.11.0"
fast_log = "1.5.24"
log = "0.4.17"
once_cell = "1.12.0"
rbatis = "4.0.7"
rbdc = "0.1.2"
rbdc-mysql = "0.1.7"
rbs = "0.1.2"
salvo = { path = "../../salvo" }
serde = { version = "1.0.143", features = ["derive"] }
tokio = { version = "1.20.1", features = ["macros"] }
tracing = "0.1.36"
tracing-subscriber = "0.3.15"
serde_json = "1.0"
```

Example 4 (rust):
```rust
#[macro_use]
extern crate rbatis;
extern crate rbdc;

use once_cell::sync::Lazy;
use rbatis::Rbatis;
use salvo::prelude::*;
use serde::{Serialize, Deserialize};
use rbdc_mysql::driver::MysqlDriver;

pub static RB: Lazy<Rbatis> = Lazy::new(|| Rbatis::new());

#[derive(Clone, Debug, Serialize, Deserialize)]
pub struct User {
    pub id: i64,
    pub username: String,
    pub password: String,
}

impl_select!(User{select_by_id(id:String) -> Option => "`where id = #{id} limit 1`"});
#[handler]
pub async fn get_user(req: &mut Request, res: &mut Response) {
    let uid = req.query::<i64>("uid").unwrap();
    let data = User::select_by_id(&mut RB.clone(), uid.to_string()).await.unwrap();
    println!("{:?}", data);
    res.render(serde_json::to_string(&data).unwrap());
}

#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();

    // mysql connect info
    let mysql_uri = "mysql://root:123456@localhost/test";
    RB.link(MysqlDriver {}, mysql_uri).await.unwrap();

    // router
    let router = Router::with_path("users").get(get_user);

    tracing::info!("Listening on http://127.0.0.1:8698");
    let acceptor = TcpListener::new("127.0.0.1:8698").bind().await; Server::new(acceptor).serve(router).await;
}
```

---

## Ecosystem

**URL:** llms-txt#ecosystem

**Contents:**
- Community Projects
- Project Showcase
- Tutorial Resources

Salvo boasts a rich ecosystem, including community-maintained modules, projects built on Salvo, and a wealth of tutorial resources. These resources can help you better utilize the Salvo framework to build high-performance web applications.

Additionally, this subdirectory shares some libraries that work well with Salvo. The Rust language is rapidly evolving, and its ecosystem is characterized by numerous small, focused libraries. We hope these commonly used libraries can help you quickly get started with the Rust ecosystem, becoming like a pocketful of handy tools from Doraemon.

## Community Projects

Feel free to submit a PR to add your project 😄

- [Socketioxide](https://github.com/Totodore/socketioxide): A Rust implementation of a socket.io server, integrated with the Tower ecosystem and Tokio stack.
- [salvo-websocket](https://gitee.com/hubert22/salvo-websocket): WebSocket utilities for the Salvo framework.
- [SalvoRsTool](https://github.com/mdddj/SalvoRsTool): A Salvo Idea RustRover plugin for quickly generating DTO, router, and service template code.
- [protect-endpoints](https://github.com/DDtKey/protect-endpoints): A collection of components to protect your endpoints.
- [salvo-captcha](https://git.4rs.nl/awiteb/salvo-captcha): A captcha middleware for the Salvo framework.
- [salvo-casbin](https://github.com/casbin-rs/salvo-casbin): Casbin access control authentication for the Salvo framework.

- [AI00 RWKV Server](https://github.com/Ai00-X/ai00_server): An inference API server based on the RWKV model.
- [Salvo Admin](https://github.com/lyqgit/salvo-admin): A Rust rapid development framework based on Salvo and Ruoyi-Vue3.
- [Salvo Admin](https://github.com/feihua/salvo-admin): An RBAC permission management system based on Salvo and rbatis.
- [Geospatial Web](https://gitlab.com/geospatialweb/rust-mvt-postgis): Rust REST API - Martin MVT Tile Server - PostGIS.
- [ALLEY](https://github.com/alley-rs/alley-transfer): Software for fast file transfer between terminals on the same network segment.
- [musicbot](https://github.com/AdrienPensart/musicbot): A Swiss Army knife for music.
- [Replex](https://github.com/lostb1t/replex): Reshape your Plex recommendations.
- [Pure Rust Instant Message (PRIM)](https://github.com/SuanCaiYv/prim): An instant messaging system implemented purely in Rust.
- [rblog](https://github.com/prabirshrestha/rblog): A blog engine written in Rust.
- [myblog](https://github.com/driftluo/myblog): A personal blog project.
- [opensound](https://github.com/opensound-org/opensound): A professional audio system engine using Salvo as the default HTTP server backend.
- [static-api](https://github.com/josejachuf/static-api-rs): A simple application that simulates a basic REST API.
- [palpo](https://github.com/palpo-matrix-server/palpo): A Rust implementation of a Matrix server.
- [yiirs](https://github.com/shenghui0779/yiirs): A Rust API rapid development scaffold (Salvo & Axum).
- [luxy](https://github.com/alley-rs/fluxy): Software for fast file transfer between terminals on the same network segment.
- [ffxiv-best-craft](https://github.com/Tnze/ffxiv-best-craft): An FF14 crafting simulator and solver with a friendly GUI.
- [System-Monitor-Server](https://github.com/SimonYen/System-Monitor-Server): A server monitoring program written in Rust.
- [mvt server](https://github.com/mvt-proj/mvt-rs): A simple, high-speed vector tile server developed in Rust using the Salvo web framework.

## Tutorial Resources

- [Rust Salvo Zero-Basics Tutorial](https://www.bilibili.com/video/BV1FS421N71D/): A beginner-friendly tutorial for Rust Salvo.
- [Building a Vocabulary Notebook with Tera and Salvo](https://www.bilibili.com/video/BV1Kg411b75s): Building a simple vocabulary notebook web application using Tera and Salvo.
- [Is salvo really the simplest rust web framework?](https://www.youtube.com/watch?v=tf9x97eTcpk): Exploring whether Salvo is the simplest Rust web framework.
- [Salvo.rs - Un Framework sencillo de Backend creado en Rust](https://www.youtube.com/watch?v=HlVf4mE8V9s): Spanish tutorial: Salvo.rs, a simple backend framework created in Rust.
- [rust + dioxus maybe](https://www.youtube.com/watch?v=_j9tNhWbp8g): A tutorial on Rust, Salvo, Dioxus Live View, and SQLx.
- [rust + dockerfile + fly](https://www.youtube.com/watch?v=IuyQxpbxjb8): A tutorial on Rust, Salvo, Dioxus Live View, SQLx, Docker, and Fly.io.

---
url: /guide/ecology/chrono.md
---

---

## Use a lightweight base image for the runtime stage

**URL:** llms-txt#use-a-lightweight-base-image-for-the-runtime-stage

FROM debian:bookworm-slim
RUN apt-get update && \
    apt-get install -y --no-install-recommends ca-certificates && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

---

## Set the container startup command

**URL:** llms-txt#set-the-container-startup-command

**Contents:**
  - Usage Instructions

CMD ["./app"]
bash
   docker build -t your-app-name .
   bash
   docker run -p 8698:8698 your-app-name
   ```

Please adjust the port number and other configurations according to your application's actual needs.

---
url: /guide/features/index.md
---

---
url: /guide/features/hello-h3.md
---

**Examples:**

Example 1 (unknown):
```unknown
### Usage Instructions

1. Save the above `Dockerfile` in your project's root directory.
2. Adjust `your_app_name` to the actual binary file name according to your project.
3. If your application requires static files (such as templates, CSS, JS, etc.), you can add corresponding `COPY` commands.
4. If your application requires environment variable configuration, you can use the `ENV` instruction.
5. Build the Docker image:
```

Example 2 (unknown):
```unknown
6. Run the container:
```

---

## Create a non-root user to run the application

**URL:** llms-txt#create-a-non-root-user-to-run-the-application

RUN useradd -ms /bin/bash appuser
USER appuser
WORKDIR /app

---

## Copy the binary file from the build stage

**URL:** llms-txt#copy-the-binary-file-from-the-build-stage

COPY --from=build /app/target/release/your_app_name ./app

---

## OpenTelemetry Integration

**URL:** llms-txt#opentelemetry-integration

OpenTelemetry is an open-source observability framework that provides a standardized approach to collecting and exporting telemetry data from applications, including traces, metrics, and logs. It helps developers monitor application performance, troubleshoot issues, and understand the behavior of distributed systems.

Key Advantages of OpenTelemetry:

- Unified APIs and tools with no vendor lock-in
- Support for multiple programming languages and backend systems
- Low-overhead data collection
- Robust support for distributed tracing

Provides middleware with OpenTelemetry support. You can refer to the [example](https://github.com/salvo-rs/salvo/tree/main/examples/otel-jaeger) in the official repository.

---
url: /guide/features/openapi.md
---

---

## Third-Party WebSocket Plugin

**URL:** llms-txt#third-party-websocket-plugin

**Contents:**
- salvo-websocket
  - Add Project Dependency
  - Define Query Parameters for WebSocket Connection
  - Implement the WebSocketHandler Trait
  - Write Connection Handling Method

### Add Project Dependency

### Define Query Parameters for WebSocket Connection

### Implement the WebSocketHandler Trait

### Write Connection Handling Method

For more details, please refer directly to the [example](https://github.com/salvo-rs/salvo/tree/main/examples/ws-chat-with-salvo-websocket).

---
url: /guide/features/timeout.md
---

**Examples:**

Example 1 (toml):
```toml
[dependencies]
salvo-websocket = "0.0.4"
```

Example 2 (rust):
```rust
#[derive(Debug, Clone, Deserialize)]
struct User {
    name: String,
    room: String,
}
```

Example 3 (rust):
```rust
impl WebSocketHandler for User {
    // Connection event
    async fn on_connected(&self, ws_id: usize, sender: UnboundedSender<Result<Message, Error>>) {
        tracing::info!("{} connected", ws_id);
        WS_CONTROLLER.write().await.join_group(self.room.clone(), sender).unwrap();
        WS_CONTROLLER.write().await.send_group(
            self.room.clone(),
            Message::text(format!("{:?} joined!", self.name)
            ),
        ).unwrap();
    }

    // Disconnection event
    async fn on_disconnected(&self, ws_id: usize) {
        tracing::info!("{} disconnected", ws_id);
    }

    // Message reception event
    async fn on_receive_message(&self, msg: Message) {
        tracing::info!("{:?} received", msg);
        let msg = if let Ok(s) = msg.to_str() {
            s
        } else {
            return;
        };
        let new_msg = format!("<User#{}>: {}", self.name, msg);
        WS_CONTROLLER.write().await.send_group(self.room.clone(), Message::text(new_msg.clone())).unwrap();
    }

    async fn on_send_message(&self, msg: Message) -> Result<Message, Error> {
        tracing::info!("{:?} sending", msg);
        Ok(msg)
    }
}
```

Example 4 (rust):
```rust
#[tokio::main]
async fn main() {
    tracing_subscriber::fmt().init();
    let router = Router::new()
        .push(Router::with_path("chat").handle(user_connected));
    tracing::info!("Listening on http://127.0.0.1:8698");
    let acceptor = TcpListener::new("127.0.0.1:8698").bind().await; Server::new(acceptor).serve(router).await;
}

#[handler]
async fn user_connected(req: &mut Request, res: &mut Response) -> Result<(), StatusError> {
    let user: Result<User, ParseError> = req.parse_queries();
    match user {
        Ok(user) => {
            WebSocketUpgrade::new().upgrade(req, res, |ws| async move {
                handle_socket(ws, user).await;
            }).await
        }
        Err(_err) => {
            Err(StatusError::bad_request())
        }
    }
}
```

---

## use salvo_oapi::ToParameters;

**URL:** llms-txt#use-salvo_oapi::toparameters;

#[derive(ToParameters, serde::Deserialize)]
#[salvo(parameters(default_parameter_in = Query))]
struct Filter {
    #[salvo(parameter(value_type = Vec<i32>))]
    id: Vec<String>
}
rust

**Examples:**

Example 1 (unknown):
```unknown
_**We can use another [`ToSchema`][to_schema] to override the field type.**_
```

---

## Rust Error Handling Libraries

**URL:** llms-txt#rust-error-handling-libraries

**Contents:**
- thiserror vs snafu
  - thiserror
  - snafu
  - Comparison
- anyhow

- [thiserror](https://docs.rs/thiserror/latest/thiserror/) provides convenient derive macros for custom error types.

- [snafu](https://docs.rs/snafu/latest/snafu/) is an error handling and reporting framework with context.

- [anyhow](https://docs.rs/anyhow/latest/anyhow/) is a flexible error handling and reporting library.

## thiserror vs snafu

thiserror is a lightweight library that provides derive macros to simplify error definitions.

- Concise syntax with low ceremony
- Suitable for creating error type libraries and APIs
- Typically used for creating libraries intended for others

snafu provides a more comprehensive error handling framework with a focus on error context and error chaining.

- Encourages more precise error context addition through the "context selector" pattern
- Promotes the "one error enum per module" pattern
- Supports both struct and tuple-style error variants
- Built-in backtrace support

| Feature            | thiserror                 | snafu                               |
| ------------------ | ------------------------- | ----------------------------------- |
| Syntax conciseness | More concise              | More verbose                        |
| Error context      | Basic support             | Rich context mechanisms             |
| Suitable scale     | Small to medium projects  | Medium to large projects            |
| Lines per error    | \~2 lines per error       | \~5 lines per error                 |
| Error organization | Usually single error enum | Encourages module-level error enums |
| Backtrace support  | No built-in support       | Built-in support                    |

**Selection advice**:

- **Choose thiserror** when you need simple and clear error types, especially in libraries
- **Choose snafu** when you need more structured error handling, particularly in large applications

anyhow is a different error handling library from the above two, focusing on applications rather than libraries.

- Designed for error handling in applications, not libraries
- Provides a dynamic `anyhow::Error` type that can contain any error implementing the `Error` trait
- Simplifies handling across multiple error types
- No need to define custom error types

**anyhow vs thiserror/snafu**:

- anyhow focuses on error handling during rapid application development
- thiserror/snafu focus on creating precise error type hierarchies
- anyhow is typically used in application code
- thiserror/snafu are typically used in library code

In practice, anyhow and thiserror are often used together: libraries use thiserror to define precise error types, while applications use anyhow to handle errors from various sources.

---
url: /donate.md
---

**Examples:**

Example 1 (rust):
```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum DataError {
    #[error("Database error: {0}")]
    DatabaseError(#[from] sqlx::Error),
    
    #[error("Validation error: {0}")]
    ValidationError(String),
    
    #[error("Record not found")]
    NotFound,
}
```

Example 2 (rust):
```rust
use snafu::{Snafu, ResultExt, Backtrace};

#[derive(Debug, Snafu)]
pub enum Error {
    #[snafu(display("Failed to read config file {filename:?}"))]
    ReadConfig {
        filename: String,
        source: std::io::Error,
        backtrace: Backtrace,
    },
    
    // Tuple style is also supported
    #[snafu(display("IO error"))]
    Io(#[snafu(source)] std::io::Error, #[snafu(backtrace)] Backtrace),
}

// Context selector example
fn read_config(path: &str) -> Result<Config, Error> {
    std::fs::read_to_string(path).context(ReadConfigSnafu { filename: path })?;
    // ...
}
```

Example 3 (rust):
```rust
use anyhow::{Context, Result};

fn main() -> Result<()> {
    let config = std::fs::read_to_string("config.json")
        .context("Failed to read config file")?;
        
    let app_config: AppConfig = serde_json::from_str(&config)
        .context("Invalid config format")?;
        
    // Using Result<T> as a type alias for Result<T, anyhow::Error>
    Ok(())
}
```

---

## Serde: Rust Serialization and Deserialization Framework

**URL:** llms-txt#serde:-rust-serialization-and-deserialization-framework

**Contents:**
- Key Features
- How It Works
- Basic Usage
  - Setting Up Dependencies
  - Using Derive Macros
  - Attribute Customization
  - Supported Data Formats
- Advanced Usage
  - Manual Trait Implementation
  - Type Mapping

[Serde](https://docs.rs/serde/latest/serde/) is a core library in the Rust ecosystem, providing an efficient and versatile framework for serialization and deserialization. Its name is derived from the combination of "**Ser**ialization" and "**De**serialization."

- **Versatility**: Supports multiple data formats such as JSON, YAML, TOML, MessagePack, and more.
- **Zero-Cost Abstraction**: Code generated at compile time is as efficient as handwritten code.
- **Flexibility**: Allows customization of serialization and deserialization behavior.
- **Strong Typing**: Leverages Rust's type system to ensure data integrity.
- **Wide Adoption**: Serves as the standard library for data exchange in the Rust ecosystem.

The core of Serde lies in its Intermediate Representation (IR) design, which divides the serialization and deserialization processes into two steps:

1. **Serialization**: Converts Rust data structures into a generic intermediate representation, then into the target format.
2. **Deserialization**: Converts input formats into the generic intermediate representation, then into Rust data structures.

This design enables the addition of new data formats without modifying applications that use Serde.

### Setting Up Dependencies

### Using Derive Macros

The most common usage involves using derive macros to automatically implement serialization and deserialization traits for structs:

### Attribute Customization

Serde provides a rich set of attributes to customize serialization behavior:

### Supported Data Formats

Serde integrates with various data formats, each with its own crate:

- **serde\_json**: JSON format
- **serde\_yaml**: YAML format
- **toml**: TOML format
- **bincode**: Binary format
- **postcard**: Space-optimized binary format
- **rmp/rmp-serde**: MessagePack format
- **ciborium**: CBOR format
- ...and other formats

### Manual Trait Implementation

For special requirements, you can manually implement the `Serialize` and `Deserialize` traits:

You can create mappings between different data representations:

## Learning and Resources

Serde is a feature-rich library, and this article only covers the basics. To fully leverage Serde, it is recommended to:

1. Visit the [Serde official documentation](https://serde.rs/) for detailed APIs and examples.
2. Check the [GitHub repository](https://github.com/serde-rs/serde) for source code and the latest updates.

As a foundational library in the Rust ecosystem, Serde provides powerful and flexible tools for data exchange. By mastering Serde, you can effortlessly handle various data exchange requirements, making your applications more robust and interoperable.

---
url: /guide/ecology/error.md
---

**Examples:**

Example 1 (toml):
```toml
[dependencies]
serde = { version = "1.0", features = ["derive"] }
serde_json = "1.0" # Or other format libraries like serde_yaml, toml, etc.
```

Example 2 (rust):
```rust
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize, Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    let point = Point { x: 1, y: 2 };

    // Convert Point to a JSON string
    let serialized = serde_json::to_string(&point).unwrap();
    println!("Serialized result: {}", serialized); // Output: {"x":1,"y":2}

    // Convert the JSON string back to Point
    let deserialized: Point = serde_json::from_str(&serialized).unwrap();
    println!("Deserialized result: {:?}", deserialized); // Output: Point { x: 1, y: 2 }
}
```

Example 3 (rust):
```rust
#[derive(Serialize, Deserialize, Debug)]
struct User {
    #[serde(rename = "user_id")]
    id: u64,
    
    #[serde(default)]
    name: String,
    
    #[serde(skip_serializing_if = "Option::is_none")]
    email: Option<String>,
    
    #[serde(skip)]
    temporary_data: usize,
}
```

Example 4 (rust):
```rust
use serde::{Serialize, Serializer, Deserialize, Deserializer};

struct MyType {
    // Fields...
}

impl Serialize for MyType {
    fn serialize<S>(&self, serializer: S) -> Result<S::Ok, S::Error>
    where
        S: Serializer,
    {
        // Custom serialization logic
    }
}

impl<'de> Deserialize<'de> for MyType {
    fn deserialize<D>(deserializer: D) -> Result<Self, D::Error>
    where
        D: Deserializer<'de>,
    {
        // Custom deserialization logic
    }
}
```

---

## Graceful Shutdown

**URL:** llms-txt#graceful-shutdown

Graceful shutdown refers to the process where, when a server is being shut down, it does not immediately terminate all connections. Instead, it first stops accepting new requests while allowing existing requests sufficient time to complete their processing before closing the service. This approach prevents requests from being abruptly interrupted, thereby improving user experience and system reliability.

Salvo provides support for graceful shutdown through the `handle` method of the `Server`, which retrieves the server handle, followed by calling the `stop_graceful` method to implement the shutdown. After invoking this method, the server will:

- Stop accepting new connection requests
- Wait for existing requests to complete processing
- Forcefully close any remaining connections after a specified timeout (if provided)

Here is a simple example:

In the example above:

- `server.handle()` retrieves the server handle, which can be used to control the server's lifecycle
- `handle.stop_graceful(None)` initiates the graceful shutdown process, where `None` indicates no timeout is set, meaning the server will wait indefinitely for all requests to complete
- To set a timeout, you can pass `Some(Duration)`, after which any remaining connections will be forcefully closed

This approach is particularly suitable for applications deployed in container environments or on cloud platforms, as well as for scenarios requiring hot updates to ensure that requests are not unexpectedly interrupted.

---
url: /guide/topics/testing.md
---

**Examples:**

Example 1 (rust):
```rust
use salvo_core::prelude::*;

#[tokio::main]
async fn main() {
    let acceptor = TcpListener::new("127.0.0.1:8698").bind().await;
    let server = Server::new(acceptor);
    let handle = server.handle();

    // Gracefully shut down the server
    tokio::spawn(async move {
        tokio::time::sleep(std::time::Duration::from_secs(60)).await;
        handle.stop_graceful(None);
    });
    server.serve(Router::new()).await;
}
```

---

## WebTransport

**URL:** llms-txt#webtransport

**Contents:**
- Use Cases
- Salvo Implementation
  - Simple Example
  - Configuration and Startup
- Complete Example

WebTransport is a network transport protocol based on HTTP/3, providing bidirectional communication capabilities between clients and servers while ensuring low latency, high throughput, and security.

WebTransport is particularly suitable for the following scenarios:

- **Real-time Applications**: Online gaming, real-time collaboration tools, video conferencing, and other applications requiring low-latency communication.
- **Large File Transfers**: Supports high-throughput data transmission, ideal for media streaming and large file uploads/downloads.
- **Multiplexed Communication**: Allows simultaneous establishment of multiple bidirectional and unidirectional data streams.
- **Datagram Communication**: Supports datagram communication without guaranteed order or reliability, suitable for scenarios with extremely high real-time requirements.

Compared to WebSocket, WebTransport offers lower latency and more flexible communication patterns, especially performing better in unstable network environments.

## Salvo Implementation

The Salvo framework provides built-in support for WebTransport, allowing developers to easily build WebTransport-based applications. Key features include:

- Support for establishing WebTransport sessions
- Support for bidirectional stream communication
- Support for unidirectional stream communication
- Support for datagram transmission
- Server-initiated communication streams

Below is a simplified example of implementing a WebTransport server using Salvo:

### Configuration and Startup

Starting a Salvo application with WebTransport support requires configuring TLS certificates and a QUIC listener:

To learn more about using WebTransport in Salvo, check out the complete example on GitHub:
[https://github.com/salvo-rs/salvo/blob/main/examples/webtransport](https://github.com/salvo-rs/salvo/blob/main/examples/webtransport)

This example includes complete implementations for both server and client, demonstrating how to handle various types of WebTransport communication.

---
url: /guide/features/craft.md
---

**Examples:**

Example 1 (rust):
```rust
#[handler]
async fn connect(req: &mut Request) -> Result<(), salvo::Error> {
    let session = req.web_transport_mut().await.unwrap();
    
    // Handle datagrams
    if let Ok(Some((_, datagram))) = session.accept_datagram().await {
        // Process received datagram
        let mut resp = BytesMut::from(&b"Response: "[..]);
        resp.put(datagram);
        session.send_datagram(resp.freeze())?;
    }
    
    // Handle bidirectional streams
    if let Ok(Some(webtransport::server::AcceptedBi::BidiStream(_, stream))) = session.accept_bi().await {
        let (send, recv) = salvo::proto::quic::BidiStream::split(stream);
        // Process bidirectional stream data
    }
    
    Ok(())
}
```

Example 2 (rust):
```rust
let cert = include_bytes!("../certs/cert.pem").to_vec();
let key = include_bytes!("../certs/key.pem").to_vec();

// Configure routing
let router = Router::new().push(Router::with_path("counter").goal(connect));

// Configure TLS
let config = RustlsConfig::new(Keycert::new().cert(cert.as_slice()).key(key.as_slice()));

// Set up listeners
let listener = TcpListener::new(("0.0.0.0", 8698)).rustls(config.clone());
let acceptor = QuinnListener::new(config, ("0.0.0.0", 8698))
    .join(listener)
    .bind()
    .await;

// Start server
Server::new(acceptor).serve(router).await;
```

---

## Error Handling

**URL:** llms-txt#error-handling

**Contents:**
- Common Error Handling Approaches in Rust Applications
  - thiserror vs. anyhow
- Error Handling in Handlers
  - Error Handling with anyhow
- Displaying Error Pages

## Common Error Handling Approaches in Rust Applications

Error handling in Rust differs from languages like Java; it lacks constructs like `try...catch`. The typical approach is to define a global error handling type at the application level:

Here, the `thiserror` library is used, which facilitates defining custom error types and simplifies code. For brevity, an `AppResult` type alias is also defined.

### thiserror vs. anyhow

In the Rust error handling ecosystem, two commonly used libraries are `thiserror` and `anyhow`:

- **thiserror**: Suitable for library developers to define clear error types. It uses derive macros to help implement the `std::error::Error` trait for custom error types while allowing you to define error representations. When building a library or providing clear error types to users, `thiserror` is the better choice.

- **anyhow**: Geared toward application developers, it provides a generic error type `anyhow::Error` that can encapsulate any error implementing the `std::error::Error` trait. It focuses more on error propagation than definition, making it particularly suitable for application-layer code. You can quickly convert various errors into `anyhow::Error`, reducing the need for boilerplate code.

In some scenarios, you might use both libraries: define error types with `thiserror` in libraries and handle and propagate these errors with `anyhow` in applications.

## Error Handling in Handlers

In Salvo, `Handler`s often encounter various errors, such as database connection errors, file access errors, network connection errors, etc. For these types of errors, the aforementioned error handling approach can be applied:

Here, `home` directly returns an `AppResult<()>`. But how should this error be displayed? We need to implement the `Writer` trait for the custom error type `AppResult`, where we can decide how to display the error:

In Salvo, a `Handler` can return a `Result`, provided that both the `Ok` and `Err` types in the `Result` implement the `Writer` trait.

### Error Handling with anyhow

Given the widespread use of anyhow, Salvo provides built-in support for `anyhow::Error`. When the `anyhow` feature is enabled, `anyhow::Error` implements the `Writer` trait and is mapped to `InternalServerError`:

To use the anyhow feature, enable Salvo's `anyhow` feature in Cargo.toml:

This allows your handler functions to directly return `anyhow::Result<T>`:

Errors often contain sensitive information, which generally shouldn't be visible to regular users for security and privacy reasons. However, if you're a developer or site administrator, you might prefer errors to be fully exposed, revealing the most accurate error information.

As shown, in the `write` method, we can access references to `Request` and `Depot`, making it convenient to implement the above approach:

## Displaying Error Pages

Salvo's built-in error pages meet requirements in most cases, displaying Html, Json, or Xml pages based on the request's data type. However, there are situations where custom error page displays are still desired.

This can be achieved by implementing a custom `Catcher`. For detailed instructions, refer to the [`Catcher`](/guide/concepts/catcher.md) section.

---
url: /guide/topics/send-file.md
---

**Examples:**

Example 1 (rust):
```rust
use thiserror::Error;

#[derive(Error, Debug)]
pub enum AppError {
    #[error("io: `{0}`")]
    Io(#[from] io::Error),
    #[error("utf8: `{0}`")]
    FromUtf8(#[from] FromUtf8Error),
    #[error("diesel: `{0}`")]
    Diesel(#[from] diesel::result::Error),
    ...
}

pub type AppResult<T> = Result<T, AppError>;
```

Example 2 (rust):
```rust
#[handler]
async fn home() -> AppResult<()> {

}
```

Example 3 (rust):
```rust
#[async_trait]
impl Writer for AppError {
    async fn write(mut self, _req: &mut Request, depot: &mut Depot, res: &mut Response) {
        res.render(Text::Plain("I'm a error, hahaha!"));
    }
}
```

Example 4 (rust):
```rust
#[cfg(feature = "anyhow")]
#[async_trait]
impl Writer for ::anyhow::Error {
    async fn write(mut self, _req: &mut Request, _depot: &mut Depot, res: &mut Response) {
        res.render(StatusError::internal_server_error());
    }
}
```

---

## use salvo_oapi::{ToParameters, ToSchema};

**URL:** llms-txt#use-salvo_oapi::{toparameters,-toschema};

#[derive(ToSchema)]
struct Id {
    value: i64,
}

#[derive(ToParameters, serde::Deserialize)]
#[salvo(parameters(default_parameter_in = Query))]
struct Filter {
    #[salvo(parameter(value_type = Id))]
    id: String
}
rust
#[derive(salvo_oapi::ToParameters, serde::Deserialize)]
struct Item {
    #[salvo(parameter(maximum = 10, minimum = 5, multiple_of = 2.5))]
    id: i32,
    #[salvo(parameter(max_length = 10, min_length = 5, pattern = "[a-z]*"))]
    value: String,
    #[salvo(parameter(max_items = 5, min_items = 1))]
    items: Vec<String>,
}
rust

**Examples:**

Example 1 (unknown):
```unknown
_**Example of attribute value validation**_
```

Example 2 (unknown):
```unknown
_**Use `schema_with` to manually implement the schema for the field.**_
```

---

## How to Deploy Applications

**URL:** llms-txt#how-to-deploy-applications

**Contents:**
- Docker Deployment

A Salvo project, after compilation, becomes an executable file. For deployment, you only need to upload this executable along with its dependent static resources to the server.

For Rust-based projects, there is also a very simple deployment platform: [shuttle.rs](https://www.shuttle.rs). Shuttle provides support for Salvo-like projects. For details, please refer to the [official documentation](https://docs.shuttle.rs/guide/salvo-examples.html).

You can also use Docker to deploy Salvo applications. Below is a basic `Dockerfile` example, which you can adjust according to your project's requirements:

---

## Reqwest: Rust HTTP Client Library

**URL:** llms-txt#reqwest:-rust-http-client-library

**Contents:**
- Basic Usage
  - Making a GET Request
  - Making a POST Request
  - Form Data
  - JSON Data
  - Response Handling
- Advanced Features
  - Redirect Policy
  - Cookie Support
  - Proxy Settings

[Reqwest](https://docs.rs/reqwest/latest/reqwest/) is a high-level HTTP client library that simplifies the HTTP request handling process and provides many commonly used features:

- Support for both asynchronous and blocking APIs
- Handling various types of request bodies: plain text, JSON, URL-encoded forms, multipart forms
- Customizable redirect policies
- HTTP proxy support
- TLS encryption enabled by default
- Cookie management

### Making a GET Request

For single requests, you can use the `get` convenience method:

> Note: If you plan to make multiple requests, it is better to create a `Client` and reuse it to take advantage of connection pooling.

### Making a POST Request

You can set the request body using the `body()` method:

Sending form data is a common requirement. You can use any type that can be serialized into form data:

You can easily send JSON data using the `json` method (requires the `json` feature):

### Response Handling

Responses can be handled in various ways:

By default, the client will automatically handle HTTP redirects, following up to 10 jumps. You can customize this behavior using `ClientBuilder`:

You can enable automatic storage and sending of session cookies via `ClientBuilder`:

System proxies are enabled by default, which will look for HTTP or HTTPS proxy settings in environment variables:

- `HTTP_PROXY` or `http_proxy`: proxy for HTTP connections
- `HTTPS_PROXY` or `https_proxy`: proxy for HTTPS connections
- `ALL_PROXY` or `all_proxy`: proxy for both types of connections

You can also explicitly set a proxy via code:

### TLS Configuration

The client uses TLS by default to connect to HTTPS targets:

You can configure timeout durations for requests:

Reqwest provides various optional features that can be enabled or disabled via Cargo features:

- `http2` (enabled by default): support for HTTP/2
- `default-tls` (enabled by default): provides TLS support for HTTPS
- `rustls-tls`: provides TLS functionality using rustls
- `blocking`: provides a blocking client API
- `json`: provides JSON serialization and deserialization functionality
- `multipart`: provides multipart form functionality
- `cookies`: provides cookie session support
- `gzip`, `brotli`, `deflate`, `zstd`: support for various response body decompression
- `socks`: provides SOCKS5 proxy support

When asynchronous operations are not needed, you can use the blocking API (requires the `blocking` feature):

---
url: /guide/ecology/serde.md
---

**Examples:**

Example 1 (rust):
```rust
let body = reqwest::get("https://www.rust-lang.org")
    .await?
    .text()
    .await?;

println!("body = {body:?}");
```

Example 2 (rust):
```rust
let client = reqwest::Client::new();
let res = client.get("https://www.rust-lang.org")
    .send()
    .await?;
```

Example 3 (rust):
```rust
let client = reqwest::Client::new();
let res = client.post("http://httpbin.org/post")
    .body("the exact body that is sent")
    .send()
    .await?;
```

Example 4 (rust):
```rust
// This will send a POST request with a body of `foo=bar&baz=quux`
let params = [("foo", "bar"), ("baz", "quux")];
let client = reqwest::Client::new();
let res = client.post("http://httpbin.org/post")
    .form(&params)
    .send()
    .await?;
```

---

## ---

**URL:** llms-txt#---

url: /guide/index.md
---

---

## Build stage

**URL:** llms-txt#build-stage

FROM rust:slim AS build
WORKDIR /app

---
