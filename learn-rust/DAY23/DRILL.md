# DAY23: Drill — "Config Parser with Serde"

Create a new Cargo project:

```bash
cargo new --bin config_parser
cd config_parser
```

Add to `Cargo.toml`:

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

## Requirements

```rust
use serde::{Serialize, Deserialize};
use std::fs;

#[derive(Serialize, Deserialize, Debug)]
struct ServerConfig {
    host: String,
    port: u16,
    #[serde(default = "default_timeout")]
    timeout_seconds: u64,
    #[serde(default = "default_log_level")]
    log_level: String,
}

fn default_timeout() -> u64 { 30 }
fn default_log_level() -> String { "info".to_string() }

fn main() -> Result<(), Box<dyn std::error::Error>> {
    // 1. Create a default config (not from JSON)
    let default_config = ServerConfig {
        host: "localhost".to_string(),
        port: 8080,
        timeout_seconds: 60,
        log_level: "debug".to_string(),
    };

    // 2. Serialize default config to pretty JSON and print
    let json = serde_json::to_string_pretty(&default_config)?;
    println!("=== Default Config ===");
    println!("{}", json);

    // 3. Write the config to a file
    fs::write("config.json", &json)?;
    println!("Config written to config.json");

    // 4. Read it back from the file
    let file_content = fs::read_to_string("config.json")?;
    let loaded_config: ServerConfig = serde_json::from_str(&file_content)?;
    println!("\n=== Loaded Config ===");
    println!("{:?}", loaded_config);

    // 5. Parse a JSON string with missing fields (to test defaults)
    let minimal_json = r#"{
        "host": "example.com",
        "port": 443
    }"#;
    let minimal_config: ServerConfig = serde_json::from_str(minimal_json)?;
    println!("\n=== Minimal Config (with defaults) ===");
    println!("{:?}", minimal_config);

    Ok(())
}
```

## Example output

```
=== Default Config ===
{
  "host": "localhost",
  "port": 8080,
  "timeout_seconds": 60,
  "log_level": "debug"
}
Config written to config.json

=== Loaded Config ===
ServerConfig { host: "localhost", port: 8080, timeout_seconds: 60, log_level: "debug" }

=== Minimal Config (with defaults) ===
ServerConfig { host: "example.com", port: 443, timeout_seconds: 30, log_level: "info" }
```

## Hints

<details>
<summary>Click for hints</summary>

- `#[serde(default = "function_name")]` calls that function when the field is missing
- `#[serde(default)]` uses `Default::default()` for the type
- Make sure the `deserialize` derive is included
- `Box<dyn std::error::Error>` lets you use `?` for both serde errors and io errors
</details>

## Bonus

Add a `DatabaseConfig` struct and nest it inside `ServerConfig`. Use `#[serde(flatten)]` to inline the fields.

## How to run

```bash
cd ~/Dev/learn-rust/DAY23/config_parser
cargo run
```

## Before you ask for review

- Does it compile?
- Does the serde_json output look right?
- Do the default values work when fields are missing?
- Check `config.json` — is it valid JSON?

## When you're done

Show me your `src/main.rs` and `config.json`.
