# DAY21: Project — Thread-Safe Task Runner

---

## The anecdote: "A thread-safe task runner is a restaurant kitchen with a ticket system"

A busy restaurant kitchen has:
- A **ticket printer** that prints orders (the shared queue)
- Multiple **chefs** who grab tickets and cook (worker threads)
- A **lock** on the spice rack so two chefs don't grab the same jar (mutex)

Today you'll build a thread-safe task runner — a queue of jobs processed by a pool of worker threads. Each job is a closure (function) that workers execute.

---

## Design

```rust
use std::sync::{mpsc, Arc, Mutex};
use std::thread;

type Job = Box<dyn FnOnce() + Send + 'static>;

struct ThreadPool {
    workers: Vec<Worker>,
    sender: mpsc::Sender<Job>,
}

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool { }

    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    { }
}

impl Drop for ThreadPool {
    fn drop(&mut self) { }
}
```

---

## Key concepts used

- **Arc\<Mutex\<T\>\>** — shared, thread-safe mutable data
- **mpsc::channel** — send jobs from main thread to workers
- **Box\<dyn FnOnce() + Send + 'static\>** — type-erased, sendable, 'static closure
- **Drop trait** — clean up threads when pool is dropped

---

## Summary

| Component | Purpose |
|-----------|---------|
| `ThreadPool` | Manages workers and job queue |
| `Worker` | Holds a thread and receives jobs |
| `Arc<Mutex<Receiver>>` | Thread-safe shared receiver |
| `Box<dyn FnOnce() + Send>` | Type-erased job closure |

This project ties together closures, threads, channels, smart pointers, and traits.

Your turn. Open DAY21/DRILL.md.
