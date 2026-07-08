# DAY26: Checkpoint

**Closed book.** No looking back.

1. What does `async fn` return?
   impl Future<Output = T>

2. What does the `.await` keyword do?
   yields control to the runtime and waits for the future to complete

3. What does `tokio::join!(a, b)` do differently than `a.await; b.await;`?
   runs both futures concurrently instead of sequentially

4. What crate provides the async runtime used with axum?
   tokio

5. What is a Future in Rust?
   a value representing an operation that may not have completed yet

---

**Check your answers against the lesson after you finish.**
