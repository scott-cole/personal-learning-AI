# DAY07: Project — CLI Todo Manager

---

## The anecdote: "A todo list is a mechanic's workbench"

A mechanic has a workbench with tasks written on sticky notes. They add tasks, mark them done, remove them, and list what's left.

Your project today builds a todo list manager in the terminal. It reinforces everything from Week 1: variables, functions, ownership, borrowing, structs, and methods.

---

## What we're building

A CLI todo manager that runs in the terminal with these commands:

```
add "Buy groceries"
add "Write Rust code"
list
complete 1
list
remove 1
list
```

---

## Design

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
```

We store items in a `Vec` and auto-increment IDs. No file persistence yet — that comes in DAY14.

---

## Key concepts you'll use

- **Structs** to model items and the list
- **impl blocks** with methods to add, list, complete, remove
- **References** — methods take `&mut self` when modifying
- **&str vs String** — the `title` parameter comes in as `&str`, stored as `String`
- **println!** with format strings

---

## Outline

```rust
fn main() {
    let mut todo = TodoList::new();

    // Hardcoded demo for now — no CLI parsing yet
    todo.add("Buy groceries");
    todo.add("Write Rust code");
    todo.list();

    todo.complete(1);
    todo.list();
}
```

---

## Summary

| Command | Method | Effect |
|---------|--------|--------|
| `add "title"` | `add(&mut self, title: &str)` | Adds new item |
| `list` | `list(&self)` | Prints all items |
| `complete N` | `complete(&mut self, id: i32)` | Marks item N done |
| `remove N` | `remove(&mut self, id: i32)` | Removes item N |

This project consolidates everything from Week 1. Take your time. Get it working.

Your turn. Open DAY07/DRILL.md.
