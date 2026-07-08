# DAY30: Drill — "Persistent URL Shortener"

Copy your DAY29 project, or create a new one:

```bash
cargo new --bin url_shortener_prod
cd url_shortener_prod
```

Add to `Cargo.toml`:

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
rand = "0.8"
```

## Requirements

Extend DAY29's URL shortener with file persistence.

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

#[derive(Clone, Serialize, Deserialize)]
struct UrlEntry {
    url: String,
    visits: u64,
}

type UrlDb = Arc<Mutex<HashMap<String, UrlEntry>>>;

#[derive(Clone)]
struct AppState {
    db: UrlDb,
    filename: String,
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

fn generate_code() -> String {
    // same as DAY29
}

// TODO: save the database to a JSON file
fn save_to_file(state: &AppState) -> Result<(), Box<dyn std::error::Error>> {
    // TODO
}

// TODO: load the database from a JSON file
fn load_from_file(filename: &str) -> HashMap<String, UrlEntry> {
    // TODO
}

async fn shorten(
    State(state): State<AppState>,
    Json(req): Json<ShortenRequest>,
) -> (StatusCode, Json<ShortenResponse>) {
    let code = generate_code();
    {
        let mut db = state.db.lock().unwrap();
        db.insert(code.clone(), UrlEntry {
            url: req.url.clone(),
            visits: 0,
        });
    }
    // TODO: save to file

    let response = ShortenResponse {
        short: code.clone(),
        url: req.url,
    };
    (StatusCode::CREATED, Json(response))
}

async fn redirect(
    State(state): State<AppState>,
    Path(code): Path<String>,
) -> Result<Redirect, StatusCode> {
    let url = {
        let mut db = state.db.lock().unwrap();
        // TODO: find url, increment visits, return url string
    };

    match url {
        Some(url) => Ok(Redirect::permanent(&url)),
        None => Err(StatusCode::NOT_FOUND),
    }
}

async fn get_stats(
    State(state): State<AppState>,
    Path(code): Path<String>,
) -> Result<Json<StatsResponse>, StatusCode> {
    let db = state.db.lock().unwrap();
    match db.get(&code) {
        Some(entry) => Ok(Json(StatsResponse {
            url: entry.url.clone(),
            visits: entry.visits,
        })),
        None => Err(StatusCode::NOT_FOUND),
    }
}

#[tokio::main]
async fn main() {
    let filename = "urls.json".to_string();

    let state = AppState {
        db: Arc::new(Mutex::new(load_from_file(&filename))),
        filename,
    };

    println!("Loaded {} URLs from file", state.db.lock().unwrap().len());

    let app = Router::new()
        .route("/shorten", post(shorten))
        .route("/:code", get(redirect))
        .route("/stats/:code", get(get_stats))
        .with_state(state);

    let listener = tokio::net::TcpListener::bind("127.0.0.1:3000")
        .await
        .unwrap();
    println!("URL Shortener (persistent) on http://127.0.0.1:3000");
    axum::serve(listener, app).await.unwrap();
}
```

## Testing with curl

```bash
# Shorten a URL
curl -X POST http://127.0.0.1:3000/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://doc.rust-lang.org/book/"}'

# Response: {"short":"Xk9mB2","url":"https://doc.rust-lang.org/book/"}

# Visit it
curl -v http://127.0.0.1:3000/Xk9mB2

# Check stats
curl http://127.0.0.1:3000/stats/Xk9mB2

# STOP the server, then restart:
cargo run

# The URL should still be available!
curl http://127.0.0.1:3000/stats/Xk9mB2

# Visit count should have been preserved
```

## Hints

<details>
<summary>Click for hints</summary>

- `save_to_file`: `let db = state.db.lock().unwrap(); let json = serde_json::to_string_pretty(&*db)?; std::fs::write(&state.filename, json)?;`
- `load_from_file`: `fs::read_to_string(filename).ok().and_then(|s| serde_json::from_str(&s).ok()).unwrap_or_default()`
- Call `save_to_file(&state).unwrap();` after inserting a new URL
- Call `save_to_file(&state).unwrap();` after incrementing visits (in the redirect handler)
- The `{ }` block in `shorten` ensures the lock is released before saving (to avoid deadlock)
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY30/url_shortener_prod
cargo run
```

Run it, add some URLs, stop it, run it again — verify data is preserved.

## Before you ask for review

- Does it compile?
- Do URLs survive a restart?
- Does `urls.json` contain valid JSON?
- Does the visit count persist across restarts?
- What happens if `urls.json` is deleted?

## When you're done

Show me your `src/main.rs`, `urls.json`, and curl output demonstrating persistence.

You've completed all 30 days! You have a production-ready URL shortener. Deploy it, extend it, or use it as a portfolio piece.
