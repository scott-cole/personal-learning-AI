# DAY20: Checkpoint

**Closed book.** No looking back.

1. How do you create a new thread in Rust?
   thread::spawn(|| { })

2. What does `handle.join().unwrap()` do?
   waits for the thread to finish and returns its result

3. Why must you use `move` with closures passed to `thread::spawn`?
   because the thread might outlive the caller, so it needs to take ownership

4. What does MPSC stand for in `mpsc::channel()`?
   multiple producer, single consumer

5. How do you send a value through a channel?
   tx.send(value)

---

**Check your answers against the lesson after you finish.**
