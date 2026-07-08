# DAY21: Checkpoint

**Closed book.** No looking back.

1. What does `Arc` provide over regular `Rc`?
   atomic reference counting for thread safety

2. Why do we wrap `mpsc::Receiver` in `Arc<Mutex<>>` for the thread pool?
   to share the single receiver across multiple threads safely

3. What does `Box<dyn FnOnce() + Send + 'static>` mean?
   a heap-allocated closure that can be called once, is safe to send between threads, and lives forever

4. Why must we call `drop(self.sender.take())` before joining worker threads in `Drop`?
   closing the sender signals workers to stop receiving, so they can exit their loops

5. What does `thread::spawn(move || { ... })` require and why?
   move closure because the thread may outlive the parent scope and needs to own its captures

---

**Check your answers against the lesson after you finish.**
