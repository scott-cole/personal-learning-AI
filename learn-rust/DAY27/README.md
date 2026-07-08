# DAY27: Logging (env_logger, log crate) and Configuration

---

## The anecdote: "Logging is a flight recorder"

A plane's flight recorder (black box) records everything — altitude, speed, engine status. When something goes wrong, engineers review the logs to understand what happened.

Logging in Rust works the same way. Your program records events at different levels of severity. When bugs happen, logs tell the story.

```rust
use log::{info, warn, error, debug};

fn main() {
    env_logger::init();

    info!("Server starting on port 8080");
    warn!("Disk space below 10%");
    error!("Failed to connect to database: timeout");
    debug!("Cache miss for key: user_42");
}
```

---

## Log levels

| Level | When to use |
|-------|-------------|
| `error!` | Something is broken — immediate attention needed |
| `warn!` | Something unusual — might be a problem |
| `info!` | Normal operation — what's happening |
| `debug!` | Detailed info for debugging |
| `trace!` | Very detailed — step-by-step execution |

---

## Setting up logging

Add to `Cargo.toml`:

```toml
[dependencies]
log = "0.4"
env_logger = "0.11"
```

In your code:

```rust
use log::{info, warn};

fn main() {
    env_logger::init();

    info!("Application started");
    // Your code here
}
```

---

## Controlling log output

Set the `RUST_LOG` environment variable:

```bash
RUST_LOG=info cargo run
RUST_LOG=debug cargo run
RUST_LOG=warn cargo run
RUST_LOG=my_crate=debug cargo run  # Only debug for my crate
```

---

## Configuration pattern

A common pattern is loading config from environment variables or files:

```rust
use std::env;

#[derive(Debug)]
struct Config {
    host: String,
    port: u16,
    database_url: String,
    log_level: String,
}

impl Config {
    fn from_env() -> Config {
        Config {
            host: env::var("HOST").unwrap_or_else(|_| "127.0.0.1".to_string()),
            port: env::var("PORT")
                .unwrap_or_else(|_| "8080".to_string())
                .parse()
                .expect("PORT must be a number"),
            database_url: env::var("DATABASE_URL")
                .expect("DATABASE_URL must be set"),
            log_level: env::var("LOG_LEVEL")
                .unwrap_or_else(|_| "info".to_string()),
        }
    }
}
```

---

## Logging in production

```rust
use log::{info, error};

fn process_order(order_id: u64) {
    info!("Processing order {}", order_id);

    match perform_charge(order_id) {
        Ok(_) => info!("Order {} charged successfully", order_id),
        Err(e) => error!("Order {} failed: {}", order_id, e),
    }
}
```

Add context to every log message — when debugging a production issue, you'll thank yourself.

---

## Summary

| Concept | Code | Purpose |
|---------|------|---------|
| Initialize logger | `env_logger::init()` | Start logging |
| Info log | `info!("message")` | Normal operations |
| Warning log | `warn!("message")` | Unusual events |
| Error log | `error!("message")` | Failures |
| Debug log | `debug!("message")` | Troubleshooting |
| Set level | `RUST_LOG=debug` | Control verbosity |
| Config from env | `env::var("KEY")` | Load configuration |

Your turn. Open DAY27/DRILL.md.
