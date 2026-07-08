# DAY24: Basic Web Server with axum

---

## The anecdote: "A web server is a restaurant — takes orders, returns food"

A restaurant has:
- A **front door** — the address (like `127.0.0.1:3000`)
- A **menu** — the API routes
- A **waiter** — the router that takes orders and delivers food
- The **kitchen** — the handler functions that prepare responses

A web server in Rust works the same way. The client walks in (makes a request), the waiter takes the order (routes to a handler), the kitchen prepares food (processes the request), and the waiter brings it back (returns a response).

---

## Setting up with axum

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
```

---

## Your first server

```rust
use axum::{Router, routing::get};

async fn hello() -> &'static str {
    "Hello, World!"
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(hello));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
    println!("Server running on http://127.0.0.1:3000");
    axum::serve(listener, app).await.unwrap();
}
```

---

## Multiple routes

```rust
use axum::{Router, routing::get};

async fn home() -> &'static str { "Welcome!" }
async fn about() -> &'static str { "About us" }
async fn contact() -> &'static str { "Contact page" }

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(home))
        .route("/about", get(about))
        .route("/contact", get(contact));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000").await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

---

## Path parameters

```rust
use axum::{Router, routing::get, extract::Path};

async fn greet_user(Path(name): Path<String>) -> String {
    format!("Hello, {}!", name)
}

// In router: .route("/user/:name", get(greet_user))
// Visit: /user/Scott → "Hello, Scott!"
```

---

## Query parameters

```rust
use axum::{Router, routing::get, extract::Query};
use serde::Deserialize;

#[derive(Deserialize)]
struct SearchParams {
    q: String,
    page: Option<u32>,
}

async fn search(Query(params): Query<SearchParams>) -> String {
    let page = params.page.unwrap_or(1);
    format!("Searching for '{}' on page {}", params.q, page)
}
```

---

## Returning JSON

```rust
use axum::{Router, routing::get, Json};
use serde::Serialize;

#[derive(Serialize)]
struct User {
    id: u64,
    name: String,
}

async fn get_user() -> Json<User> {
    Json(User {
        id: 1,
        name: "Scott".to_string(),
    })
}
```

---

## Summary

| Concept | Code | Description |
|---------|------|-------------|
| Route handler | `async fn handler() -> &str` | Handles requests |
| Define route | `.route("/path", get(handler))` | Map URL to handler |
| Path param | `Path(name): Path<String>` | Extract from URL |
| Query param | `Query(params): Query<T>` | Extract ?key=val |
| JSON response | `Json(response)` | Return JSON |
| Run server | `axum::serve(listener, app)` | Start serving |

Your turn. Open DAY24/DRILL.md.
