# DAY28: Drill — "CLI Todo with clap"

Create a new Cargo project:

```bash
cargo new --bin cli_todo
cd cli_todo
```

Add to `Cargo.toml`:

```toml
[dependencies]
clap = { version = "4", features = ["derive"] }
serde = { version = "1", features = ["derive"] }
serde_json = "1"
```

## Requirements

Build a CLI todo manager with subcommands using clap.

```rust
use clap::{Parser, Subcommand};
use serde::{Serialize, Deserialize};
use std::fs;

#[derive(Serialize, Deserialize)]
struct Todo {
    items: Vec<TodoItem>,
}

#[derive(Serialize, Deserialize)]
struct TodoItem {
    id: u64,
    title: String,
    done: bool,
}

#[derive(Parser)]
#[command(name = "todo")]
#[command(about = "A simple todo manager", version = "1.0")]
struct Cli {
    #[command(subcommand)]
    command: Commands,
}

#[derive(Subcommand)]
enum Commands {
    /// Add a new todo item
    Add {
        /// Title of the todo
        title: String,
    },
    /// List all todo items
    List,
    /// Mark a todo item as done
    Done {
        /// ID of the todo to mark done
        id: u64,
    },
    /// Remove a todo item
    Remove {
        /// ID of the todo to remove
        id: u64,
    },
}

fn main() {
    let cli = Cli::parse();
    let filename = "todos.json";
    let mut todo = load_todo(filename);

    match cli.command {
        Commands::Add { title } => {
            // TODO: create new TodoItem, push to todo.items, save
            println!("Added: {}", title);
        }
        Commands::List => {
            // TODO: list all items with their ID and status
            // Format: [ID] [✓/✗] Title
        }
        Commands::Done { id } => {
            // TODO: find item with id and set done = true
            println!("Marked {} as done", id);
        }
        Commands::Remove { id } => {
            // TODO: remove item with id
            println!("Removed {}", id);
        }
    }

    save_todo(&todo, filename);
}

fn load_todo(filename: &str) -> Todo {
    // TODO: read from file or return empty Todo
}

fn save_todo(todo: &Todo, filename: &str) {
    // TODO: write to file as pretty JSON
}
```

## Usage

```bash
# Add todos
cargo run -- add "Buy groceries"
cargo run -- add "Finish Rust course"
cargo run -- add "Walk the dog"

# List todos
cargo run -- list

# Mark done
cargo run -- done 2

# Remove
cargo run -- remove 1

# Get help
cargo run -- --help
cargo run -- add --help
```

## Example output

```
$ cargo run -- add "Buy groceries"
Added: Buy groceries

$ cargo run -- list
1 [✗] Buy groceries
2 [✗] Finish Rust course
3 [✗] Walk the dog

$ cargo run -- done 2
Marked 2 as done

$ cargo run -- list
1 [✗] Buy groceries
2 [✓] Finish Rust course
3 [✗] Walk the dog
```

## Hints

<details>
<summary>Click for hints</summary>

- `load_todo`: `fs::read_to_string(filename).map(|s| serde_json::from_str(&s).unwrap()).unwrap_or(Todo { items: vec![] })`
- `save_todo`: `fs::write(filename, serde_json::to_string_pretty(todo).unwrap())`
- For Add: create TodoItem with `id = todo.items.len() as u64 + 1`
- For List: iterate `todo.items` and print formatted
- For Done: `todo.items.iter_mut().find(|i| i.id == id).map(|i| i.done = true)`
- For Remove: `todo.items.retain(|i| i.id != id)`
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY28/cli_todo
cargo run -- add "Test todo"
cargo run -- list
```

## Before you ask for review

- Does it compile?
- Do all subcommands work?
- Does `--help` show proper documentation?
- Are todos persisted between runs?
- What happens if you try to done/remove a non-existent ID?

## When you're done

Show me your `src/main.rs`.
