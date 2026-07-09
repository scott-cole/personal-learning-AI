# DAY21: Drill — "Thread Pool"

Create a new Cargo project:

```bash
cargo new --bin thread_pool
cd thread_pool
```

## Requirements

Build a `ThreadPool` that can execute closures across multiple worker threads.

```rust
use std::sync::{mpsc, Arc, Mutex};
use std::thread;
use std::time::Duration;

type Job = Box<dyn FnOnce() + Send + 'static>;

struct ThreadPool {
    workers: Vec<Worker>,
    sender: Option<mpsc::Sender<Job>>,
}

struct Worker {
    id: usize,
    thread: Option<thread::JoinHandle<()>>,
}

impl ThreadPool {
    fn new(size: usize) -> ThreadPool {
        // TODO: create channel, spawn workers
        let mut workers = Vec::with_capacity(size);
        let (sender, receiver) = mpsc::channel();
        let receiver = Arc::new(Mutex::new(receiver));

        for id in 0..size {
            workers.push(Worker::new(id, Arc::clone(&receiver)));
        }

        ThreadPool {
            workers,
            sender: Some(sender),
        }
    }

    fn execute<F>(&self, f: F)
    where
        F: FnOnce() + Send + 'static,
    {
        // TODO: send the job through the channel
        let job = Box::new(f);
        self.sender.as_ref().unwrap().send(job).unwrap();
    }
}

impl Drop for ThreadPool {
    fn drop(&mut self) {
        // TODO: drop sender to close channel
        // TODO: join all worker threads
        drop(self.sender.take());

        for worker in &mut self.workers {
            if let Some(thread) = worker.thread.take() {
                thread.join().unwrap();
            }
        }
    }
}

impl Worker {
    fn new(id: usize, receiver: Arc<Mutex<mpsc::Receiver<Job>>>) -> Worker {
        // TODO: spawn a thread that loops receiving jobs
        let thread = thread::spawn(move || loop {
            let job = receiver.lock().unwrap().recv();
            match job {
                Ok(job) => {
                    println!("Worker {} executing job", id);
                    job();
                }
                Err(_) => {
                    println!("Worker {} shutting down", id);
                    break;
                }
            }
        });

        Worker {
            id,
            thread: Some(thread),
        }
    }
}

fn main() {
    let pool = ThreadPool::new(4);

    for i in 1..=8 {
        pool.execute(move || {
            println!("Task {} started", i);
            thread::sleep(Duration::from_millis(500));
            println!("Task {} finished", i);
        });
    }

    // When pool goes out of scope, Drop waits for all workers
    println!("All tasks submitted, waiting for completion...");
}
```

## Example output

```
All tasks submitted, waiting for completion...
Worker 0 executing job
Worker 1 executing job
Worker 2 executing job
Worker 3 executing job
Task 3 started
Task 2 started
Task 1 started
Task 4 started
Task 4 finished
Task 1 finished
Task 3 finished
Task 2 finished
Worker 1 executing job
Worker 3 executing job
Worker 0 executing job
Worker 2 executing job
Task 5 started
Task 6 started
Task 7 started
Task 8 started
Task 6 finished
Task 8 finished
Task 7 finished
Task 5 finished
Worker 0 shutting down
Worker 1 shutting down
Worker 3 shutting down
Worker 2 shutting down
```

## Hints

<details>
<summary>Click for hints</summary>

- `Worker::new` spawns a thread with `thread::spawn(move || { ... })`
- Inside the thread, loop calling `receiver.lock().unwrap().recv()`
- `recv()` returns `Err` when the channel is closed — break out of the loop
- `Drop` for ThreadPool: drop the sender, then join all threads
- `Option<thread::JoinHandle<()>>` lets you `.take()` the handle to call `.join()`
- `Option<mpsc::Sender<Job>>` lets you drop the sender by calling `.take()`
</details>

## How to run

```bash
cd ~/Dev/personal-learning-AI/learn-rust/DAY21/thread_pool
cargo run
```

## Before you ask for review

- Does it compile?
- Are all 8 tasks executed?
- Do all workers shut down cleanly?
- Does the program exit without hanging?
- What happens if you don't drop the sender before joining?

## When you're done

Show me your `src/main.rs`.
