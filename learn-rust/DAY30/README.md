# DAY30: Final Project — URL Shortener with Persistence

---

## The anecdote: "Persistence is a filing cabinet"

Your DAY29 URL shortener was like a waiter's memory — fast, but everything is forgotten when the restaurant closes.

Today you're adding a **filing cabinet** — a JSON file that stores all the short URLs permanently. Open the cabinet tomorrow and everything is still there.

This is the same pattern you learned on DAY14 (persistent todo list) applied to your final project.

---

## What we're adding

- Save URL mappings to a JSON file on every change
- Load URL mappings from the JSON file on startup
- Use `serde_json` for persistence

---

## Design changes

```rust
use serde::{Serialize, Deserialize};

#[derive(Clone, Serialize, Deserialize)]
struct UrlEntry {
    url: String,
    visits: u64,
}

struct AppState {
    db: Arc<Mutex<HashMap<String, UrlEntry>>>,
    filename: String,
}
```

---

## Save function

```rust
fn save_to_file(state: &AppState) -> Result<(), Box<dyn std::error::Error>> {
    let db = state.db.lock().unwrap();
    let json = serde_json::to_string_pretty(&*db)?;
    std::fs::write(&state.filename, json)?;
    Ok(())
}
```

Call this after every mutation (shorten and redirect).

---

## Load function

```rust
fn load_from_file(filename: &str) -> HashMap<String, UrlEntry> {
    std::fs::read_to_string(filename)
        .ok()
        .and_then(|s| serde_json::from_str(&s).ok())
        .unwrap_or_default()
}
```

---

## Technology stack

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
rand = "0.8"
```

---

## Summary

This is your final project. It combines everything you've learned:

- **Variables, functions, structs** — DAY01-06
- **Ownership and borrowing** — DAY03-04
- **Enums and pattern matching** — DAY08
- **Error handling with Result** — DAY09
- **Vectors and HashMaps** — DAY10
- **Generics and traits** — DAY11-12
- **File I/O** — DAY22
- **Serde** — DAY23
- **Web server with axum** — DAY24-25
- **Async/await** — DAY26

You now have a production-ready URL shortener. The code is clean, the state is persistent, and all endpoints work.

Your turn. Open DAY30/DRILL.md.
