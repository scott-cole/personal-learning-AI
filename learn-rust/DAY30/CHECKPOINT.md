# DAY30: Checkpoint

**Closed book.** No looking back.

1. What does `serde_json::to_string_pretty(&*db)` do?
   serializes the HashMap to a pretty-printed JSON string

2. Why do we use a `{ }` block when mutating the database before saving to file?
   to release the Mutex lock before doing file I/O, avoiding a potential deadlock

3. What function reads a file and returns its content as a String, returning Result?
   std::fs::read_to_string()

4. What does `.unwrap_or_default()` return if the `Result` is `Err`?
   the default value for the type (e.g., empty HashMap)

5. How did this project demonstrate persistence?
   by saving URL mappings to a JSON file and loading them on restart, surviving process restarts

---

**Check your answers against the lesson after you finish.**
