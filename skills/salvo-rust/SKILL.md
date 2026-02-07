---
name: salvo-rust
description: Salvo - 用 Rust 编写的简单而强大的 Web 框架。统一的 Handler/Middleware 设计，灵活的路由系统，支持 OpenAPI、JWT、CORS、限流等丰富功能。
---

# Salvo Web 框架

Salvo 是一个用 Rust 编写的极简而强大的 Web 框架，基于 Hyper 和 Tokio。它的设计理念是简单易用，同时提供生产级的性能和功能。

## 核心特性

- **统一设计**: Handler 和 Middleware 统一，都通过 `hoop` 添加到 Router
- **灵活路由**: 支持平面和树形路由定义，路径参数、正则匹配、通配符
- **简化参数**: Handler 参数可选，可以省略不需要的参数
- **自动提取**: 自动从请求中提取和验证参数
- **OpenAPI**: 自动生成 API 文档
- **丰富中间件**: JWT、CORS、限流、压缩、日志等
- **类型安全**: 充分利用 Rust 类型系统

## 何时使用此技能

- 使用 Salvo 框架开发 Rust Web 应用
- 定义路由和处理器
- 编写中间件
- 处理请求和响应
- 集成 OpenAPI 文档
- 实现认证和授权

## 快速开始

### 安装

```toml
[dependencies]
salvo = "0.73"
tokio = { version = "1", features = ["macros"] }
```

### Hello World

```rust
use salvo::prelude::*;

#[handler]
async fn hello() -> &'static str {
    "Hello World"
}

#[tokio::main]
async fn main() {
    let router = Router::new().get(hello);
    let acceptor = TcpListener::new("127.0.0.1:5800").bind().await;
    Server::new(acceptor).serve(router).await;
}
```

## Handler 定义

### 基本 Handler

```rust
// 完整参数
#[handler]
async fn hello(req: &mut Request, depot: &mut Depot, res: &mut Response) {
    res.render("Hello");
}

// 省略不需要的参数
#[handler]
async fn hello(res: &mut Response) {
    res.render("Hello");
}

// 参数顺序可以任意
#[handler]
async fn hello(res: &mut Response, req: &mut Request) {
    res.render("Hello");
}
```

### 返回值

```rust
// 返回字符串
#[handler]
async fn hello() -> &'static str {
    "Hello World"
}

// 返回 JSON
#[handler]
async fn user() -> Json<User> {
    Json(User { name: "John".into() })
}

// 返回 Result
#[handler]
async fn get_user(req: &Request) -> Result<Json<User>, StatusError> {
    let id = req.param::<i64>("id")?;
    let user = fetch_user(id).await?;
    Ok(Json(user))
}
```

## 路由系统

### 平面路由

```rust
Router::new()
    .get(list_articles)
    .post(create_article);

Router::new()
    .path("articles/{id}")
    .get(show_article)
    .patch(edit_article)
    .delete(delete_article);
```

### 树形路由（推荐）

```rust
Router::new()
    .push(
        Router::new()
            .path("articles")
            .get(list_articles)
            .post(create_article)
            .push(
                Router::new()
                    .path("{id}")
                    .get(show_article)
                    .patch(edit_article)
                    .delete(delete_article)
            )
    );
```

### 路径参数

```rust
// 基本参数
Router::new().path("users/{id}");

// 正则限制
Router::new().path(r"users/{id:\d+}");

// 数字类型
Router::new().path("users/<id:num>");      // 任意位数
Router::new().path("users/<id:num[10]>");  // 恰好 10 位
Router::new().path("users/<id:num(3..10)>"); // 3-9 位
Router::new().path("users/<id:num(3..=10)>"); // 3-10 位

// 通配符
Router::new().path("files/{**rest}");  // 匹配所有，可为空
Router::new().path("files/{*+rest}");  // 匹配所有，不能为空
Router::new().path("files/{*?rest}");  // 匹配单段，可为空

// 组合表达式
Router::new().path("article_{id:num}");
Router::new().path("images/{name}.{ext}");
```

### 获取路径参数

```rust
#[handler]
async fn show_article(req: &Request) -> String {
    let id = req.param::<i64>("id").unwrap();
    format!("Article {}", id)
}
```

## 中间件

### 添加中间件

```rust
// Middleware 就是 Handler
#[handler]
async fn auth_check(req: &Request, depot: &mut Depot, res: &mut Response, ctrl: &mut FlowCtrl) {
    if !is_authenticated(req) {
        res.status_code(StatusCode::UNAUTHORIZED);
        ctrl.skip_rest(); // 跳过后续处理
    }
}

// 通过 hoop 添加
Router::new()
    .hoop(auth_check)
    .path("admin")
    .get(admin_page);
```

### 中间件作用域

```rust
// 公开路由
let public_router = Router::new()
    .path("articles")
    .get(list_articles)
    .push(Router::new().path("{id}").get(show_article));

// 需要认证的路由
let protected_router = Router::new()
    .path("articles")
    .hoop(auth_check)  // 只影响这个分支
    .post(create_article)
    .push(
        Router::new()
            .path("{id}")
            .patch(edit_article)
            .delete(delete_article)
    );

// 合并
Router::new()
    .push(public_router)
    .push(protected_router);
```

## 请求处理

### 提取参数

```rust
#[handler]
async fn create_user(req: &mut Request) -> Result<String, StatusError> {
    // 路径参数
    let id = req.param::<i64>("id")?;
    
    // 查询参数
    let page = req.query::<i32>("page").unwrap_or(1);
    
    // 表单数据
    let form_data = req.form::<UserForm>().await?;
    
    // JSON 数据
    let json_data = req.parse_json::<User>().await?;
    
    Ok("Created".into())
}
```

### 自动提取（推荐）

```rust
use salvo::oapi::extract::*;

#[endpoint]
async fn create_user(
    user: JsonBody<User>,
    query: QueryParam<Pagination>,
) -> Result<Json<User>, StatusError> {
    // user 和 query 自动提取和验证
    Ok(Json(user.into_inner()))
}
```

### 文件上传

```rust
#[handler]
async fn upload(req: &mut Request) -> Result<String, StatusError> {
    let file = req.file("file").await?;
    let dest = format!("uploads/{}", file.name().unwrap_or("file"));
    tokio::fs::copy(file.path(), &dest).await?;
    Ok(format!("Uploaded to {}", dest))
}
```

## 响应处理

### 基本响应

```rust
#[handler]
async fn hello(res: &mut Response) {
    res.render("Hello World");
}

// 设置状态码
#[handler]
async fn not_found(res: &mut Response) {
    res.status_code(StatusCode::NOT_FOUND);
    res.render("Not Found");
}

// 设置 Header
#[handler]
async fn with_header(res: &mut Response) {
    res.add_header("X-Custom", "value", true).unwrap();
    res.render("OK");
}
```

### JSON 响应

```rust
#[handler]
async fn user_json() -> Json<User> {
    Json(User { name: "John".into() })
}
```

### 重定向

```rust
#[handler]
async fn redirect(res: &mut Response) {
    res.render(Redirect::found("/new-path"));
}
```

## OpenAPI 集成

### 启用 OpenAPI

```rust
use salvo::oapi::extract::*;
use salvo::prelude::*;

#[endpoint(tags("users"))]
async fn create_user(
    user: JsonBody<User>,
) -> Result<Json<User>, StatusError> {
    Ok(Json(user.into_inner()))
}

#[tokio::main]
async fn main() {
    let router = Router::new()
        .push(Router::with_path("users").post(create_user));
    
    let doc = OpenApi::new("My API", "1.0")
        .merge_router(&router);
    
    let router = router
        .push(doc.into_router("/api-doc/openapi.json"))
        .push(SwaggerUi::new("/api-doc/openapi.json").into_router("swagger-ui"));
    
    let acceptor = TcpListener::new("127.0.0.1:5800").bind().await;
    Server::new(acceptor).serve(router).await;
}
```

## 常用中间件

### CORS

```rust
use salvo::cors::Cors;

let cors = Cors::new()
    .allow_origin("https://example.com")
    .allow_methods(vec!["GET", "POST"])
    .into_handler();

Router::new().hoop(cors);
```

### JWT 认证

```rust
use salvo::jwt_auth::{JwtAuth, JwtClaims};

let auth_handler = JwtAuth::new("secret_key".to_owned())
    .finders(vec![
        Box::new(HeaderFinder::new()),
        Box::new(QueryFinder::new("token")),
    ])
    .into_handler();

Router::new().hoop(auth_handler);
```

### 限流

```rust
use salvo::rate_limiter::{RateLimiter, FixedGuard};

let limiter = RateLimiter::new(
    FixedGuard::new(),
    MokaStore::new(),
);

Router::new().hoop(limiter);
```

### 日志

```rust
use salvo::logging::Logger;

Router::new().hoop(Logger::new());
```

## Depot (共享数据)

```rust
// 在中间件中存储数据
#[handler]
async fn auth_middleware(depot: &mut Depot, ctrl: &mut FlowCtrl) {
    let user = authenticate().await;
    depot.insert("user", user);
}

// 在 Handler 中获取数据
#[handler]
async fn protected_handler(depot: &Depot) -> String {
    let user = depot.get::<User>("user").unwrap();
    format!("Hello, {}", user.name)
}
```

## 错误处理

```rust
use salvo::http::StatusError;

#[handler]
async fn may_fail() -> Result<String, StatusError> {
    if some_condition {
        Ok("Success".into())
    } else {
        Err(StatusError::internal_server_error())
    }
}

// 自定义错误
#[derive(Debug)]
struct MyError;

impl Writer for MyError {
    async fn write(mut self, _req: &mut Request, _depot: &mut Depot, res: &mut Response) {
        res.status_code(StatusCode::BAD_REQUEST);
        res.render("Custom error");
    }
}
```

## 参考文件

详细文档位于 `references/` 目录：

- `getting_started.md` - 快速入门
- `concepts.md` - 核心概念 (30 页)
- `routing.md` - 路由系统
- `features.md` - 功能特性
- `other.md` - 其他文档

## 资源链接

- 官网: https://salvo.rs/
- GitHub: https://github.com/salvo-rs/salvo
- API 文档: https://docs.rs/salvo/
- 示例: https://github.com/salvo-rs/salvo/tree/main/examples
