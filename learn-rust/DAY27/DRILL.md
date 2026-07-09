# DAY27: Drill — "Configurable HTTP Server with Logging"

Create a new Cargo project:

```bash
cargo new --bin config_server
cd config_server
```

Add to `Cargo.toml`:

```toml
[dependencies]
axum = "0.8"
tokio = { version = "1", features = ["full"] }
log = "0.4"
env_logger = "0.11"
serde = { version = "1", features = ["derive"] }
```

## Requirements

Build a configurable HTTP server with structured logging.

```rust
use axum::{Router, routing::get, response::Json};
use log::{info, warn, error};
use serde::Serialize;
use std::env;

#[derive(Debug, Serialize)]
struct Config {
    host: String,
    port: u16,
    log_level: String,
}

impl Config {
    fn from_env() -> Self {
        // TODO: load from environment variables with defaults
        // HOST defaults to "127.0.0.1"
        // PORT defaults to 3000
        // LOG_LEVEL defaults to "info"
    }
}

#[derive(Serialize)]
struct HealthResponse {
    status: String,
    version: String,
    config: Config,
}

async fn health_handler() -> Json<HealthResponse> {
    // TODO: return health info
}

async fn index_handler() -> &'static str {
    // TODO: log that someone visited the index
    "Hello from Config Server!"
}

#[tokio::main]
async fn main() {
    // TODO: initialize env_logger
    // TODO: load config
    // TODO: set RUST_LOG from config.log_level (env_logger reads RUST_LOG)
    // Actually, env_logger reads RUST_LOG at init time, so set it first

    // Hint: set env var before initializing logger
    // env::set_var("RUST_LOG", &config.log_level);

    let config = Config::from_env();
    info!("Starting server with config: {:?}", config);

    let app = Router::new()
        .route("/", get(index_handler))
        .route("/health", get(health_handler));

    let addr = format!("{}:{}", config.host, config.port);
    info!("Listening on {}", addr);

    let listener = tokio::net::TcpListener::bind(&addr).await.unwrap();
    axum::serve(listener, app).await.unwrap();
}
```

## Example output

```bash
# Run with:
RUST_LOG=info cargo run

# Should see:
[2024-01-15T10:00:00Z INFO  config_server] Starting server with config: Config { host: "127.0.0.1", port: 3000, log_level: "info" }
[2024-01-15T10:00:00Z INFO  config_server] Listening on 127.0.0.1:3000
```

## Testing

```bash
# Test the health endpoint:
curl http://127.0.0.1:3000/health
# {"status":"ok","version":"0.1.0","config":{"host":"127.0.0.1","port":3000,"log_level":"info"}}

# Test the index:
curl http://127.0.0.1:3000/
# Hello from Config Server!
```

## Hints

<details>
<summary>Click for hints</summary>

- `Config::from_env()`: use `env::var("HOST").unwrap_or_else(|_| "127.0.0.1".to_string())`
- For port: parse the string with `.parse().expect("PORT must be a number")`
- `env::set_var("RUST_LOG", &config.log_level)` must be called BEFORE `env_logger::init()`
- `health_handler` returns `Json(HealthResponse { status: "ok".into(), version: env!("CARGO_PKG_VERSION").into(), config })` — but config needs to be accessible somehow. Either store it in app state or hardcode for now
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY27/config_server
RUST_LOG=debug cargo run
```

Try different log levels: `RUST_LOG=warn`, `RUST_LOG=info`.

## Before you ask for review

- Does it compile?
- Do logs appear at the correct level?
- Does changing `RUST_LOG` change what's printed?
- Does the health endpoint return the config?

## When you're done

Show me your `src/main.rs` and output at different log levels.
