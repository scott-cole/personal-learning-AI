# DAY09: Error Handling (Result, Option, unwrap, ?, expect)

---

## The anecdote: "Result is a package that might be broken; Option is a maybe-empty box"

Imagine ordering a package online.

When it arrives, you check the box. If it's undamaged, you open it and get your item. If it's damaged, you file a complaint.

That's **Result**: a box that might contain a value OR an error.

```rust
enum Result<T, E> {
    Ok(T),   // The package is fine
    Err(E),  // The package is broken — here's the complaint
}
```

Now imagine a different box — you ordered something, but you're not sure if it's in stock. The box might contain the item, or it might be **empty**.

That's **Option**: a box that might contain a value or be empty.

```rust
enum Option<T> {
    Some(T),  // We have the item
    None,     // It's empty
}
```

---

## Handling Result

```rust
use std::fs::File;

fn main() {
    let file = File::open("hello.txt");

    let file = match file {
        Ok(f) => f,
        Err(e) => panic!("Failed to open file: {}", e),
    };
}
```

But writing `match` for every Result gets tedious. Rust gives us shortcuts.

---

## unwrap

`unwrap()` returns the value if `Ok`, or panics if `Err`:

```rust
let file = File::open("hello.txt").unwrap();
// Panics if file doesn't exist — use only in prototypes or when you're SURE it won't fail
```

**Warning:** Using `unwrap` in production is like smashing the package open. If it's broken, everything explodes.

---

## expect

`expect()` is like `unwrap` but with a custom message:

```rust
let file = File::open("hello.txt")
    .expect("Failed to open hello.txt");
// Panics with your message + the original error
```

---

## The ? operator

The `?` operator is the idiomatic way to handle errors in Rust. It says "if this is an Err, return it to the caller. If it's Ok, give me the value."

```rust
use std::fs::File;
use std::io::{self, Read};

fn read_username() -> Result<String, io::Error> {
    let mut file = File::open("username.txt")?;  // if Err, return early
    let mut username = String::new();
    file.read_to_string(&mut username)?;          // if Err, return early
    Ok(username.trim().to_string())
}
```

Each `?` is a mini match that propagates errors. The function must return a `Result` (or `Option`).

---

## Combinators: map, and_then, unwrap_or

```rust
// map: transform the Ok value
let len = File::open("file.txt")
    .map(|f| f.metadata().map(|m| m.len()));

// unwrap_or: provide a default on Err
let port = "PORT".parse::<u16>().unwrap_or(8080);

// and_then: chain fallible operations
let data = File::open("file.txt")
    .and_then(|mut f| {
        let mut s = String::new();
        f.read_to_string(&mut s).map(|_| s)
    });
```

---

## Summary

| Tool | What it does | Use when |
|------|-------------|----------|
| `match` | Full control | Every error path matters |
| `unwrap()` | Extract or panic | Prototypes, tests, infallible ops |
| `expect()` | Extract or panic with message | Better panic messages |
| `?` | Propagate error | Most production code |
| `unwrap_or(default)` | Default on error | You have a sensible default |
| `map()` | Transform Ok | Chain success path |

Your turn. Open DAY09/DRILL.md.
