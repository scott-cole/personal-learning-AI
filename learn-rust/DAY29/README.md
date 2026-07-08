# DAY29: Final Project — URL Shortener

---

## The anecdote: "A URL shortener is a phone book"

A phone book maps names to phone numbers. You look up "John's Pizza" and find "555-0100". The name is short and memorable, the number is long.

A URL shortener does the same thing: it maps short codes (like `abc123`) to long URLs (like `https://example.com/very/long/path?with=params`).

---

## What we're building

A URL shortener service with:

- `POST /shorten` — submit a long URL, get a short code back
- `GET /:code` — redirect to the original URL
- `GET /stats/:code` — see how many times a short URL was visited
- In-memory storage (DAY30 adds file persistence)

---

## Design

```rust
use std::sync::{Arc, Mutex};
use std::collections::HashMap;

struct AppState {
    urls: Arc<Mutex<HashMap<String, UrlEntry>>>,
}

#[derive(Clone)]
struct UrlEntry {
    url: String,
    visits: u64,
}
```

The short code is a random 6-character string generated from base62 (a-z, A-Z, 0-9).

---

## Technology stack

- **axum** — HTTP framework
- **tokio** — async runtime
- **serde** — JSON serialization
- **rand** — random code generation (or a simple custom implementation)

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
rand = "0.8"
```

---

## Routes

| Method | Route | What it does |
|--------|-------|-------------|
| POST | `/shorten` | Body: `{ "url": "..." }` → `{ "short": "abc123" }` |
| GET | `/:code` | Redirect to original URL (301) |
| GET | `/stats/:code` | Returns `{ "url": "...", "visits": N }` |

---

## Summary

| Component | Responsibility |
|-----------|---------------|
| POST /shorten | Generate code, store URL, return code |
| GET /:code | Look up code, increment visits, redirect |
| GET /stats/:code | Look up code, return URL and visit count |
| HashMap | In-memory storage |
| Arc<Mutex<>> | Thread-safe shared state |

This is your final project. Take your time, make it clean, and get every route working.

Your turn. Open DAY29/DRILL.md.
