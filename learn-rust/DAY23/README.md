# DAY23: Serde (Serialization/Deserialization with JSON)

---

## The anecdote: "Serde is a universal translator"

Imagine the Universal Translator from Star Trek. Someone speaks Klingon, and everyone hears it in their own language. It translates **between formats** seamlessly.

Serde (Serialization + Deserialization) is Rust's universal translator for data formats. It converts between Rust structs and JSON, YAML, TOML, BSON, and more.

```rust
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct Person {
    name: String,
    age: i32,
    email: String,
}

// Person ←→ JSON
// Person ←→ YAML
// Person ←→ TOML
// Serde translates ALL of them!
```

---

## Adding serde to your project

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

---

## Serialization (Rust → JSON)

```rust
use serde::{Serialize, Deserialize};
use serde_json;

#[derive(Serialize, Deserialize, Debug)]
struct User {
    id: u64,
    username: String,
    active: bool,
}

fn main() -> Result<(), serde_json::Error> {
    let user = User {
        id: 1,
        username: "scott".to_string(),
        active: true,
    };

    let json = serde_json::to_string_pretty(&user)?;
    println!("{}", json);

    Ok(())
}
```

Output:
```json
{
  "id": 1,
  "username": "scott",
  "active": true
}
```

---

## Deserialization (JSON → Rust)

```rust
let json_str = r#"
{
    "id": 1,
    "username": "scott",
    "active": true
}
"#;

let user: User = serde_json::from_str(json_str)?;
println!("{:?}", user);  // User { id: 1, username: "scott", active: true }
```

---

## Renaming fields

```rust
#[derive(Serialize, Deserialize)]
struct Config {
    #[serde(rename = "db_host")]
    database_host: String,

    #[serde(rename = "db_port")]
    database_port: u16,
}
```

---

## Optional fields

```rust
#[derive(Serialize, Deserialize, Debug)]
struct Config {
    host: String,
    port: u16,
    #[serde(default)]
    debug: bool,  // defaults to false if not present in JSON
}
```

---

## Working with JSON values dynamically

```rust
use serde_json::Value;

let data = serde_json::from_str(r#"{"name": "Scott", "age": 30}"#)?;
let value: Value = data;

println!("Name: {}", value["name"]);  // "Scott"
println!("Age: {}", value["age"]);    // 30
```

---

## Summary

| Operation | Function | Description |
|-----------|----------|-------------|
| Rust → JSON string | `serde_json::to_string(&val)?` | Serialize |
| Rust → Pretty JSON | `serde_json::to_string_pretty(&val)?` | Formatted JSON |
| JSON string → Rust | `serde_json::from_str(&str)?` | Deserialize |
| Rename field | `#[serde(rename = "name")]` | Custom field name |
| Default value | `#[serde(default)]` | Use Default::default() |
| Dynamic JSON | `serde_json::Value` | Untyped JSON access |

Your turn. Open DAY23/DRILL.md.
