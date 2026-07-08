# DAY14: Drill — "Persistent Todo Manager"

Create a new Cargo project and implement a todo list that saves to disk.

```bash
cargo new --bin todo_persist
cd todo_persist
```

Add serde to `Cargo.toml`, then write your code.

## Requirements

```rust
use serde::{Serialize, Deserialize};
use std::error::Error;

#[derive(Serialize, Deserialize, Debug)]
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

impl TodoList {
    fn new() -> TodoList {
        TodoList {
            items: Vec::new(),
            next_id: 1,
        }
    }

    fn add(&mut self, title: &str) {
        // TODO
    }

    fn list(&self) {
        // TODO
    }

    fn complete(&mut self, id: i32) {
        // TODO
    }

    fn remove(&mut self, id: i32) {
        // TODO
    }

    // TODO: Save to JSON file
    fn save(&self, filename: &str) -> Result<(), Box<dyn Error>> {
        // 1. Serialize self to pretty JSON string
        // 2. Write to file
    }

    // TODO: Load from JSON file
    fn load(filename: &str) -> Result<TodoList, Box<dyn Error>> {
        // 1. Read file to string
        // 2. Deserialize to TodoList
    }
}

fn main() -> Result<(), Box<dyn Error>> {
    let filename = "todos.json";

    // Try to load existing todos, or create new
    let mut todo = TodoList::load(filename).unwrap_or_else(|_| TodoList::new());

    todo.add("Buy groceries");
    todo.add("Finish Rust project");
    todo.add("Walk the dog");

    // Complete item 2
    todo.complete(2);

    todo.list();

    // Save before exiting
    todo.save(filename)?;
    println!("\nSaved to {}. Run again to see persistence!", filename);

    Ok(())
}
```

## Example output (first run)

```
[1] [✗] Buy groceries
[2] [✓] Finish Rust project
[3] [✗] Walk the dog

Saved to todos.json!
```

## Example output (second run — adds 3 more to existing)

```
[1] [✗] Buy groceries
[2] [✓] Finish Rust project
[3] [✗] Walk the dog
[4] [✗] Buy groceries
[5] [✗] Finish Rust project
[6] [✗] Walk the dog

Saved to todos.json!
```

## Hints

<details>
<summary>Click for hints</summary>

- `add` creates TodoItem with `next_id`, increments it, pushes to items
- `list` iterates items and prints formatted output (same as DAY07)
- `complete` uses `iter_mut().find()`
- `remove` uses `retain()`
- `save`: `serde_json::to_string_pretty(&self)?` then `std::fs::write(filename, json)?`
- `load`: `std::fs::read_to_string(filename)?` then `serde_json::from_str(&json)?`
- `Box<dyn Error>` lets you use `?` with any error type
- The first line of main should try to load, or create new if file doesn't exist
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY14/todo_persist
cargo run
```

Run it twice to verify persistence.

## Before you ask for review

- Does it compile?
- Run it twice — are the items from run 1 still there on run 2?
- Check `todos.json` — is it valid JSON?
- What happens if you delete `todos.json` and run again?

## When you're done

Show me your `src/main.rs` and your `todos.json`.
