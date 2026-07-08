# DAY07: Drill — "Todo Manager"

Create a new Cargo project for this one (it's big enough):

```bash
cargo new --bin todo_manager
cd todo_manager
```

Then write your code in `src/main.rs`.

## Requirements

```rust
struct TodoItem {
    id: i32,
    title: String,
    completed: bool,
}

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
        // TODO: create a TodoItem with the current next_id,
        // increment next_id, push to items
    }

    fn list(&self) {
        // TODO: print all items in format:
        // [ID] [✓/✗] Title
        // Example: [1] [✗] Buy groceries
    }

    fn complete(&mut self, id: i32) {
        // TODO: find the item with the given id and set completed = true
        // Use items.iter_mut().find(|item| item.id == id)
    }

    fn remove(&mut self, id: i32) {
        // TODO: remove the item with the given id
        // Use items.retain(|item| item.id != id)
        // Or find the index and use remove()
    }
}

fn main() {
    let mut todo = TodoList::new();

    todo.add("Buy groceries");
    todo.add("Write Rust code");
    todo.add("Learn ownership");
    todo.list();

    todo.complete(2);
    println!("\nAfter completing item 2:");
    todo.list();

    todo.remove(1);
    println!("\nAfter removing item 1:");
    todo.list();

    // Extra: try completing a non-existent ID
    todo.complete(99);
}
```

## Example output

```
[1] [✗] Buy groceries
[2] [✗] Write Rust code
[3] [✗] Learn ownership

After completing item 2:
[1] [✗] Buy groceries
[2] [✓] Write Rust code
[3] [✗] Learn ownership

After removing item 1:
[2] [✓] Write Rust code
[3] [✗] Learn ownership
```

## Hints

<details>
<summary>Click for hints</summary>

- For `list`, use `for item in &self.items { }`
- Print checkmark: `if item.completed { "✓" } else { "✗" }`
- For `complete`, use `self.items.iter_mut().find(|i| i.id == id)` and set `.completed = true`
- For `remove`, use `self.items.retain(|i| i.id != id)`
- `retain` keeps items where the closure returns `true`
- When the ID isn't found, it's fine to silently do nothing
</details>

## How to run

```bash
cd ~/Dev/learn-rust/DAY07/todo_manager
cargo run
```

## Before you ask for review

- Does `cargo run` produce the expected output?
- Does completing a non-existent ID crash? (It shouldn't)
- Are you using proper `&self` vs `&mut self`?
- Is the output formatted neatly?

## When you're done

Show me your `src/main.rs`. We'll extend this project on DAY14 with file persistence.
