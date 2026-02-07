# Salvo-Rust - Features

**Pages:** 3

---

## Cross-Origin Control

**URL:** llms-txt#cross-origin-control

**Contents:**
- What is the Same-Origin Policy?
- Why is CORS Needed?
- How CORS Works
- Using CORS in Salvo
- Main Configuration Options

CORS (Cross-Origin Resource Sharing) is a mechanism that allows browsers to make requests to cross-origin servers, thereby overcoming the restrictions imposed by the browser's same-origin policy.

## What is the Same-Origin Policy?

The same-origin policy is a security feature of browsers that restricts how documents or scripts loaded from one origin can interact with resources from another origin. "Same origin" refers to having the same protocol, domain, and port number.

## Why is CORS Needed?

CORS support is required when a frontend application needs to access APIs from a different origin. For example:

- The frontend application is deployed at `https://frontend.com`
- The API service is deployed at `https://api.backend.com`

Without CORS, the browser will prevent the frontend application from accessing the API service.

CORS implements cross-origin access control through a series of HTTP headers:

- **Simple Requests**: Sent directly, with the server controlling whether access is allowed via response headers.
- **Preflight Requests**: The browser first sends an OPTIONS request to ask the server if cross-origin access is allowed, and only sends the actual request after receiving permission.

Since the browser sends preflight requests with `Method::OPTIONS`, it is necessary to handle such requests. The CORS middleware must be added to the `Service`.

## Using CORS in Salvo

Salvo provides built-in CORS middleware that can be easily configured and used. Here is an example code snippet:

:::warning
Note: The `.hoop(cors)` middleware is applied to the `Service`, not the `Router`. This ensures automatic handling of OPTIONS preflight requests.

## Main Configuration Options

The CORS middleware provides various configuration options:

- **Allowed Origins**: Controls which domains can access resources.
- **Allowed Methods**: Specifies allowed HTTP methods (GET, POST, etc.).
- **Allowed Headers**: Specifies allowed request headers.
- **Exposed Headers**: Specifies which response headers can be accessed by the client.
- **Allow Credentials**: Determines whether requests can include credentials such as cookies.
- **Preflight Request Cache Time**: The cache duration for preflight request results.

By configuring CORS appropriately, you can ensure both security and the ability to meet cross-origin access requirements.

---
url: /guide/features/csrf.md
---

**Examples:**

Example 1 (rust):
```rust
let cors = Cors::new()
    .allow_origin(["http://127.0.0.1:8698", "http://localhost:8698"])
    .allow_methods(vec![Method::GET, Method::POST, Method::DELETE])
    .allow_headers("authorization")
    .into_handler();

// Set up backend router with CORS protection
let router = Router::with_path("hello").post(hello);
let service = Service::new(router).hoop(cors);
```

Example 2 (unknown):
```unknown
**Cargo.toml**
```

---

## JWT Authentication

**URL:** llms-txt#jwt-authentication

JWT (JSON Web Token) is an open standard (RFC 7519) for securely transmitting information between parties. It is a compact, URL-safe method of representing claims in JSON object format, commonly used for authentication and information exchange.

A JWT consists of three parts:

- **Header** - Specifies the token type and the signing algorithm used
- **Payload** - Contains claims, such as user ID, roles, expiration time, etc.
- **Signature** - Used to verify that the message was not altered during transmission

Typical usage of JWT in authentication flow:

1. After user login, the server generates a JWT token
2. The token is returned to the client and stored (typically in localStorage or cookies)
3. In subsequent requests, the client includes this token in the Authorization header
4. The server validates the token's authenticity and grants access

Provides middleware for JWT Auth authentication.

---
url: /guide/features/logging.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

---

## Craft Feature

**URL:** llms-txt#craft-feature

**Contents:**
- Use Cases
- Basic Usage
  - Creating a Service Struct
  - Creating Handler Functions
  - Creating Endpoints
  - Static Endpoints
- Parameter Extractors
- Integration with OpenAPI
- Complete Example

Craft allows developers to automatically generate handler functions and endpoints through simple annotations, while seamlessly integrating with OpenAPI documentation generation.

The Craft feature is particularly useful in the following scenarios:

- When you need to quickly create route handler functions from struct methods
- When you want to reduce boilerplate code for manual parameter extraction and error handling
- When you need to automatically generate OpenAPI documentation for your API
- When you want to decouple business logic from the web framework

To use the Craft feature, you need to import the following modules:

### Creating a Service Struct

Annotate your impl block with the `#[craft]` macro to convert struct methods into handler functions or endpoints.

### Creating Handler Functions

Use `#[craft(handler)]` to convert a method into a handler function:

This method will become a handler function that:

- Automatically extracts `left` and `right` values from query parameters
- Accesses the `state` from the struct
- Returns the calculation result as a string response

### Creating Endpoints

Use `#[craft(endpoint)]` to convert a method into an endpoint:

Endpoints can utilize `Arc` to share state, which is particularly useful when handling concurrent requests.

You can also create static endpoints that don't depend on instance state:

In this example, a custom error response description is added, which will be reflected in the generated OpenAPI documentation.

## Parameter Extractors

Salvo's `oapi::extract` module provides various parameter extractors, with the most common including:

- `QueryParam<T>`: Extracts parameters from query strings
- `PathParam<T>`: Extracts parameters from URL paths
- `FormData<T>`: Extracts parameters from form data
- `JsonBody<T>`: Extracts parameters from JSON request bodies

These extractors automatically handle parameter parsing and type conversion, greatly simplifying the writing of handler functions.

## Integration with OpenAPI

The Craft feature can automatically generate API documentation compliant with the OpenAPI specification. In the example:

With this configuration, the API documentation will be available at the `/api-doc/openapi.json` endpoint, and Swagger UI will be accessible at the `/swagger-ui` path.

Below is a complete example demonstrating how to use the Craft feature to create three different types of endpoints:

---
url: /guide/ecology.md
---

**Examples:**

Example 1 (rust):
```rust
use salvo::oapi::extract::*;
use salvo::prelude::*;
```

Example 2 (rust):
```rust
#[derive(Clone)]
pub struct Opts {
    state: i64,
}

#[craft]
impl Opts {
    // Constructor
    fn new(state: i64) -> Self {
        Self { state }
    }
    
    // More methods...
}
```

Example 3 (rust):
```rust
#[craft(handler)]
fn add1(&self, left: QueryParam<i64>, right: QueryParam<i64>) -> String {
    (self.state + *left + *right).to_string()
}
```

Example 4 (rust):
```rust
#[craft(endpoint)]
pub(crate) fn add2(
    self: ::std::sync::Arc<Self>,
    left: QueryParam<i64>,
    right: QueryParam<i64>,
) -> String {
    (self.state + *left + *right).to_string()
}
```

---
