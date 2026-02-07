# Salvo-Rust - Routing

**Pages:** 2

---

## Trailing Slash

**URL:** llms-txt#trailing-slash

**Contents:**
- Use Cases

A middleware for automatically adding or removing trailing `/`.

The Trailing Slash middleware is particularly useful in the following scenarios:

- **URL Normalization**: Ensures all URLs follow a consistent format (by uniformly adding or removing trailing slashes), which helps improve SEO and avoid duplicate content issues.

- **Simplified Route Handling**: Eliminates the need to write separate routing logic for URLs with and without trailing slashes, as the middleware automatically handles this conversion.

- **Compatibility**: Some clients may automatically add or remove trailing slashes from URLs. This middleware ensures that requests are correctly routed.

- **Redirect Management**: Can be configured to automatically redirect URLs with trailing slashes to those without (or vice versa), enhancing the user experience.

- **Avoiding Route Conflicts**: In many web frameworks, `/path` and `/path/` may be treated as different routes. This middleware can handle them uniformly.

---
url: /guide/features/websocket.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

---

## Reverse Proxy

**URL:** llms-txt#reverse-proxy

A reverse proxy is a server architecture that receives requests from clients and forwards them to one or more backend servers. Unlike a forward proxy (which acts on behalf of clients), a reverse proxy operates on behalf of the server side.

Key advantages of reverse proxies:

- Load Balancing: Distributes requests across multiple servers
- Enhanced Security: Hides real server information
- Content Caching: Improves performance
- Path Rewriting and Forwarding: Routes requests flexibly

The Salvo framework provides middleware for reverse proxy functionality.

---
url: /guide/features/rate-limiter.md
---

**Examples:**

Example 1 (unknown):
```unknown
**Cargo.toml**
```

---
