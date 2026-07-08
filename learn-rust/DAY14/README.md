# DAY14: Project — CLI Task Manager with File Persistence

---

## The anecdote: "Persistence is a refrigerator"

Your DAY07 todo list was like a whiteboard — write tasks, erase them, but when you wipe the board, everything's gone.

Today you're adding a **refrigerator** — a place where your tasks stay even when the power goes out (your program closes). Open the fridge tomorrow, and yesterday's groceries are still there.

File persistence in Rust works the same way: write data to a file, read it back on startup.

---

## What we're building

Extend DAY07's todo manager to save and load tasks from a JSON file on disk.

We'll use:
- `std::fs` for file operations
- `serde_json` for serialization (JSON reading/writing)
- `Result<(), Box<dyn Error>>` for error handling

---

## Setting up

Create a new Cargo project:

```bash
cargo new --bin todo_persist
cd todo_persist
```

Add to `Cargo.toml`:

```toml
[dependencies]
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

---

## Design

```rust
use serde::{Serialize, Deserialize};

#[derive(Serialize, Deserialize)]
struct TodoItem {
    id: i32,
    title: String,
    completed: bool,
}

#[derive(Serialize, Deserialize)]
struct TodoList {
    items: Vec<TodoItem>,
    next_id: i32,
}
```

The `#[derive(Serialize, Deserialize)]` attributes tell `serde` how to convert these structs to/from JSON. It's like adding a "this can be frozen" label to your leftovers.

---

## Key methods to add

```rust
impl TodoList {
    fn save(&self, filename: &str) -> Result<(), Box<dyn Error>> {
        let json = serde_json::to_string_pretty(self)?;
        std::fs::write(filename, json)?;
        Ok(())
    }

    fn load(filename: &str) -> Result<TodoList, Box<dyn Error>> {
        let json = std::fs::read_to_string(filename)?;
        let list: TodoList = serde_json::from_str(&json)?;
        Ok(list)
    }
}
```

---

## Summary

This project combines:
- Structs with serde derives
- File I/O with `std::fs`
- Error handling with `?`
- `Box<dyn Error>` for flexible error types
- Everything from Week 1 and 2

Your turn. Open DAY14/DRILL.md.
