# DAY20: Concurrency (std::thread, Channels)

---

## The anecdote: "Threads are multiple chefs in a kitchen; channels are a conveyor belt between them"

Imagine a restaurant kitchen. One chef can cook one dish at a time. Add a second chef — now they can work in parallel, each handling different orders.

But they need to communicate: "Plate this dish!" "Need more onions!" That's where a **channel** comes in — like a conveyor belt between stations.

- A **thread** is a chef — a separate flow of execution
- A **channel** is the conveyor belt — sends messages from one thread to another

---

## Spawning threads

```rust
use std::thread;
use std::time::Duration;

fn main() {
    let handle = thread::spawn(|| {
        for i in 1..10 {
            println!("Thread: {}", i);
            thread::sleep(Duration::from_millis(1));
        }
    });

    for i in 1..5 {
        println!("Main: {}", i);
        thread::sleep(Duration::from_millis(1));
    }

    handle.join().unwrap();  // Wait for the thread to finish
}
```

---

## Move closures with threads

When you use variables from the parent scope in a thread, you need `move`:

```rust
use std::thread;

fn main() {
    let data = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("{:?}", data);  // data is moved into the thread
    });

    // println!("{:?}", data);  // ERROR! data was moved

    handle.join().unwrap();
}
```

Without `move`, the closure would borrow `data`, but the thread might outlive the main function. `move` ensures ownership is transferred.

---

## Channels: std::sync::mpsc

MPSC = Multiple Producer, Single Consumer. Like many chefs sending orders to one printer.

```rust
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let vals = vec![1, 2, 3];
        for val in vals {
            tx.send(val).unwrap();
            thread::sleep(std::time::Duration::from_millis(100));
        }
    });

    for received in rx {
        println!("Got: {}", received);
    }
}
```

---

## Multiple producers

Clone the transmitter for multiple producers:

```rust
let (tx, rx) = mpsc::channel();

let tx1 = tx.clone();
thread::spawn(move || {
    tx1.send("from thread 1").unwrap();
});

let tx2 = tx.clone();
thread::spawn(move || {
    tx2.send("from thread 2").unwrap();
});

for msg in rx {
    println!("{}", msg);
}
```

---

## Summary

| Concept | Syntax | Purpose |
|---------|--------|---------|
| Spawn thread | `thread::spawn(|| { })` | Create new thread |
| Join handle | `handle.join().unwrap()` | Wait for thread |
| Move closure | `move \|\| { }` | Move data into thread |
| Create channel | `mpsc::channel()` | Returns (tx, rx) |
| Send message | `tx.send(val)` | Send to channel |
| Receive message | `rx.recv()` or `for in rx` | Receive from channel |

Your turn. Open DAY20/DRILL.md.
