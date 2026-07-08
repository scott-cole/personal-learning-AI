# DAY14: Checkpoint

**Closed book.** No looking back.

1. What does `#[derive(Serialize, Deserialize)]` do?
   automatically generates serialization/deserialization code for a struct

2. What crate provides JSON serialization for Rust?
   serde_json

3. What does `std::fs::write(filename, data)` do?
   writes data to a file, creating or overwriting it

4. What type is `Box<dyn Error>` useful for?
   returning any type of error from a function

5. What does `.unwrap_or_else(|_| TodoList::new())` do?
   tries to unwrap the Result; if Err, calls the closure to create a new TodoList

---

**Check your answers against the lesson after you finish.**
