# DAY29: Drill — "URL Shortener"

Create a new Cargo project:

```bash
cargo new --bin url_shortener
cd url_shortener
```

Add to `Cargo.toml`:

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
rand = "0.8"
```

## Requirements

```rust
use axum::{
    Router, routing::{get, post},
    extract::{State, Path},
    response::{Json, Redirect},
    http::StatusCode,
};
use serde::{Serialize, Deserialize};
use std::sync::{Arc, Mutex};
use std::collections::HashMap;

#[derive(Clone, Serialize)]
struct UrlEntry {
    url: String,
    visits: u64,
}

type UrlDb = Arc<Mutex<HashMap<String, UrlEntry>>>;

#[derive(Clone)]
struct AppState {
    db: UrlDb,
}

#[derive(Deserialize)]
struct ShortenRequest {
    url: String,
}

#[derive(Serialize)]
struct ShortenResponse {
    short: String,
    url: String,
}

#[derive(Serialize)]
struct StatsResponse {
    url: String,
    visits: u64,
}

// TODO: generate a random 6-character code (a-z, A-Z, 0-9)
fn generate_code() -> String {
    // Use rand to pick random characters
    // Hint: rand::Rng, sample from a static byte array
    "abcdef".to_string()  // placeholder
}

// TODO: POST /shorten — accept JSON with url field, return short code
async fn shorten(
    State(state): State<AppState>,
    Json(req): Json<ShortenRequest>,
) -> (StatusCode, Json<ShortenResponse>) {
    // TODO
}

// TODO: GET /:code — redirect to the original URL
async fn redirect(
    State(state): State<AppState>,
    Path(code): Path<String>,
) -> Result<Redirect, StatusCode> {
    // TODO: look up code, increment visits, redirect 301
}

// TODO: GET /stats/:code — return URL and visit count
async fn get_stats(
    State(state): State<AppState>,
    Path(code): Path<String>,
) -> Result<Json<StatsResponse>, StatusCode> {
    // TODO
}

#[tokio::main]
async fn main() {
    let state = AppState {
        db: Arc::new(Mutex::new(HashMap::new())),
    };

    let app = Router::new()
        .route("/shorten", post(shorten))
        .route("/:code", get(redirect))
        .route("/stats/:code", get(get_stats))
        .with_state(state);

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000")
        .await
        .unwrap();
    println!("URL Shortener on http://127.0.0.1:3000");
    axum::serve(listener, app).await.unwrap();
}
```

## Testing with curl

```bash
# Shorten a URL
curl -X POST http://127.0.0.1:3000/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://www.rust-lang.org/learn"}'

# Response: {"short":"aB3xK9","url":"https://www.rust-lang.org/learn"}

# Visit the short URL (follow redirect)
curl -v http://127.0.0.1:3000/aB3xK9
# Should show: Location: https://www.rust-lang.org/learn

# Check stats
curl http://127.0.0.1:3000/stats/aB3xK9
# {"url":"https://www.rust-lang.org/learn","visits":1}
```

## Hints

<details>
<summary>Click for hints</summary>

- `generate_code`: Use `rand::thread_rng()` and pick random bytes from `b"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"`
- `shorten`: generate code, create `UrlEntry`, insert into db, return `(StatusCode::CREATED, Json(...))`
- `redirect`: lock db, find entry, increment visits, return `Ok(Redirect::permanent(&entry.url))`
- `get_stats`: lock db, find entry, return `Ok(Json(...))` or `Err(StatusCode::NOT_FOUND)`
- `Redirect::permanent` creates a 308 (permanent) redirect
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY29/url_shortener
cargo run
```

Test with curl in another terminal.

## Before you ask for review

- Does `POST /shorten` return a short code?
- Does visiting the short URL redirect to the original?
- Does visiting the short URL increment the visit count?
- Does `GET /stats/:code` show the correct visit count?
- Does a non-existent code return 404?

## When you're done

Show me your `src/main.rs` and curl output for all three endpoints.
