# DAY26: Async/Await in Rust (tokio, futures)

---

## The anecdote: "Async/await is a juggler — keeps multiple balls in the air"

A juggler doesn't wait for one ball to land before throwing the next. They throw ball 1, then while it's in the air, they throw ball 2, then ball 3, then catch ball 1, and so on.

Async/await in Rust works the same way. While waiting for one operation (like a network request or file read), the program can work on something else. It doesn't block — it **juggles**.

---

## The async keyword

```rust
async fn fetch_data() -> String {
    // This function runs concurrently — it yields control
    // when it hits an .await point
    "data".to_string()
}
```

An `async fn` returns a `Future` — it doesn't run until you `.await` it or spawn it.

---

## The await keyword

```rust
async fn main() {
    let result = fetch_data().await;
    println!("{}", result);
}
```

`.await` says: "Run this future until it completes. While waiting, let other tasks run."

---

## What is a Future?

A `Future` is a value that represents an operation that may not be complete yet. It's like a ticket for a concert — the concert hasn't happened yet, but the ticket represents the promise that it will.

```rust
use std::future::Future;

// fetch_data() returns impl Future<Output = String>
let future: impl Future<Output = String> = fetch_data();
// Nothing has happened yet!
let result = future.await;  // NOW it runs
```

---

## Running async tasks concurrently

```rust
use tokio::time::{sleep, Duration};

async fn task(name: &str, seconds: u64) -> String {
    sleep(Duration::from_secs(seconds)).await;
    format!("{} done after {}s", name, seconds)
}

#[tokio::main]
async fn main() {
    // Run tasks CONCURRENTLY (not sequentially)
    let (a, b) = tokio::join!(
        task("Task A", 2),
        task("Task B", 1),
    );

    println!("{}", a);  // Task A done after 2s
    println!("{}", b);  // Task B done after 1s
    // Total time: ~2 seconds, NOT 3!
}
```

---

## tokio::spawn

For fire-and-forget tasks:

```rust
#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        // This runs on the tokio runtime
        for i in 1..=5 {
            println!("Spawned: {}", i);
            tokio::time::sleep(Duration::from_millis(100)).await;
        }
    });

    // Do other work while spawned task runs
    println!("Main is free!");

    handle.await.unwrap();
}
```

---

## Async vs sync

```rust
// Synchronous — blocks the thread
fn read_file_sync() -> String {
    std::fs::read_to_string("file.txt").unwrap()
}

// Asynchronous — doesn't block (but needs tokio)
async fn read_file_async() -> String {
    tokio::fs::read_to_string("file.txt").await.unwrap()
}
```

Use async for I/O-bound work (network, file, database). Use sync for CPU-bound work.

---

## Summary

| Concept | Code | What it does |
|---------|------|-------------|
| Define async fn | `async fn name() -> T` | Returns Future |
| Await a future | `future.await` | Wait for completion |
| Join futures | `tokio::join!(a, b)` | Run concurrently |
| Spawn task | `tokio::spawn(future)` | Background execution |
| Async sleep | `sleep(dur).await` | Non-blocking sleep |
| Async runtime | `#[tokio::main]` | Entry point |

Your turn. Open DAY26/DRILL.md.
