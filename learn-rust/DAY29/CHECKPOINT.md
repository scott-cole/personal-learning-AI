# DAY29: Checkpoint

**Closed book.** No looking back.

1. What HTTP status code is used for a permanent redirect?
   301 (or 308)

2. How do you access a path parameter like `/:code` in axum?
   Path(code): Path<String>

3. What does `(StatusCode::CREATED, Json(data))` return to the client?
   201 status code with a JSON body

4. What data structure stores the URL mappings in this project?
   HashMap<String, UrlEntry>

5. Why do we use `Arc<Mutex<HashMap>>` instead of just `HashMap`?
   to allow shared mutable access across multiple threads safely

---

**Check your answers against the lesson after you finish.**
