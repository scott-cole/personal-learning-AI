# DAY24: Drill — "Simple Web Server"

Create a new Cargo project:

```bash
cargo new --bin web_server
cd web_server
```

Add to `Cargo.toml`:

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
```

## Requirements

Build a web server with these routes:

```rust
use axum::{
    Router,
    routing::get,
    response::Json,
    extract::Path,
};
use serde::Serialize;

#[derive(Serialize)]
struct Info {
    message: String,
    status: String,
}

// TODO: handler for GET / that returns "Hello from Rust!"
async fn root() -> &'static str {
    "Hello from Rust!"
}

// TODO: handler for GET /health that returns JSON:
// { "message": "OK", "status": "healthy" }
async fn health() -> Json<Info> {
    // TODO
}

// TODO: handler for GET /hello/:name that returns "Hello, [name]!"
async fn greet(Path(name): Path<String>) -> String {
    // TODO
}

// TODO: handler for GET /square/:num that returns the square as JSON
// { "number": 5, "square": 25 }
async fn square(Path(num): Path<i32>) -> Json<serde_json::Value> {
    // TODO
}

#[tokio::main]
async fn main() {
    let app = Router::new()
        .route("/", get(root))
        .route("/health", get(health))
        .route("/hello/:name", get(greet))
        .route("/square/:num", get(square));

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000")
        .await
        .unwrap();
    println!("Server running on http://127.0.0.1:3000");
    axum::serve(listener, app).await.unwrap();
}
```

## Expected responses

```bash
# Test with curl:
curl http://127.0.0.1:3000/
# Hello from Rust!

curl http://127.0.0.1:3000/health
# {"message":"OK","status":"healthy"}

curl http://127.0.0.1:3000/hello/Scott
# Hello, Scott!

curl http://127.0.0.1:3000/square/7
# {"number":7,"square":49}
```

## Hints

<details>
<summary>Click for hints</summary>

- `health()` returns `Json(Info { message: "OK".into(), status: "healthy".into() })`
- `greet()` returns `format!("Hello, {}!", name)`
- `square()` returns `Json(serde_json::json!({ "number": num, "square": num * num }))`
- Install curl if you don't have it, or test in a browser
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY24/web_server
cargo run
```

In another terminal, test with: `curl http://127.0.0.1:3000/`

## Before you ask for review

- Does `cargo run` start without errors?
- Does each endpoint return the expected response?
- What happens if you visit `/hello/` without a name?
- Try `/square/abc` — what error do you get?

## When you're done

Show me your `src/main.rs` and the curl output for each route.
