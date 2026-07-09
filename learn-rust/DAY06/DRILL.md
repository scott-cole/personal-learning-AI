# DAY06: Drill — "Build a Library System"

Create `main.rs` in `DAY06/`.

## Requirements

Build a library book tracking system with structs and methods.

```rust
struct Book {
    title: String,
    author: String,
    pages: i32,
    checked_out: bool,
}

impl Book {
    // TODO: create a new Book (all books start as not checked out)
    fn new(title: String, author: String, pages: i32) -> Book {
        // TODO
    }

    // TODO: check out the book — sets checked_out to true
    fn check_out(&mut self) {
        // TODO
    }

    // TODO: return the book — sets checked_out to false
    fn return_book(&mut self) {
        // TODO
    }

    // TODO: returns a formatted string like:
    // "Title by Author (pages pages) — available/checked out"
    fn status(&self) -> String {
        // TODO
    }
}

fn main() {
    // TODO: create a new Book
    // TODO: print its status
    // TODO: check it out
    // TODO: print its status again
    // TODO: return it
    // TODO: print its status
}
```

## Example output

```
The Rust Programming Language by Steve Klabnik (560 pages) — available
The Rust Programming Language by Steve Klabnik (560 pages) — checked out
The Rust Programming Language by Steve Klabnik (560 pages) — available
```

## Hints

<details>
<summary>Click for hints</summary>

- `new` returns `Book { title, author, pages, checked_out: false }`
- `check_out` uses `&mut self` and sets `self.checked_out = true`
- For `status`, use `if self.checked_out { "checked out" } else { "available" }`
- Use `format!` to build the string in `status`
</details>

## Bonus

Add a `check_out_duration` field (i32, days) and a method `is_overdue(&self, days_borrowed: i32) -> bool`.

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY06
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Does the status change correctly after check out and return?
- Did you remember `&mut self` for methods that modify?

## When you're done

Show me your `main.rs`.
