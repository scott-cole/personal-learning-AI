# DAY20: Drill — "Parallel Worker Pool"

Create `main.rs` in `DAY20/`.

## Requirements

Build a parallel worker pool that processes jobs across multiple threads using channels.

```rust
use std::sync::mpsc;
use std::thread;
use std::time::Duration;

fn main() {
    let (tx, rx) = mpsc::channel();
    let num_workers = 4;

    // TODO: Create 4 worker threads
    // Each worker receives jobs from the channel and processes them
    // A job is a string message
    // Simulate work with thread::sleep

    // TODO: Send 10 jobs to the workers
    // Each job should be a String like "Job 1", "Job 2", etc.

    // TODO: Wait for all workers to finish

    println!("All jobs completed!");
}

// Worker function — each worker runs this in its own thread
fn worker(id: i32, rx: mpsc::Receiver<String>) {
    // TODO: loop receiving jobs
    // When the channel is closed (sender dropped), exit
    // Print: "Worker {} processing: {}" for each job
    // Simulate 1 second of work per job
}
```

## Example output

```
Worker 1 processing: Job 1
Worker 2 processing: Job 2
Worker 3 processing: Job 3
Worker 4 processing: Job 4
Worker 1 processing: Job 5
Worker 2 processing: Job 6
Worker 3 processing: Job 7
Worker 4 processing: Job 8
Worker 1 processing: Job 9
Worker 2 processing: Job 10
All jobs completed!
```

(The order may vary — that's the beauty of concurrency!)

## Hints

<details>
<summary>Click for hints</summary>

- Create workers in a loop: `for id in 1..=num_workers`
- Each worker gets its own receiver: you need to clone the transmitter, not the receiver
- Actually, MPSC means multiple producers, single consumer. You need to share the receiver somehow.
- Solution: wrap the receiver in an `Arc<Mutex<>>` or use `rx.iter()` with cloned transmitters
- Simpler approach: use a loop that spawns threads, each cloning the tx for sending results back
- For receiving, wrap rx in `Arc<Mutex<mpsc::Receiver<String>>>` and clone the Arc for each worker
- Or: the original tx sends jobs, workers send results back on a second channel
</details>

## Bonus

Add a "result" channel: workers send `"Worker {} finished {}"` back to main, and main prints results.

## How to run

```bash
cd ~/Dev/learn-rust/DAY20
rustc main.rs && ./main
```

## Before you ask for review

- Does it compile?
- Are all 10 jobs processed?
- Does the program exit cleanly when all jobs are done?
- What happens if a worker panics?

## When you're done

Show me your `main.rs`.
