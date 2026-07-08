# DAY24: Checkpoint

**Closed book.** No looking back.

1. What macro do you use to mark main as async with tokio?
   #[tokio::main]

2. How do you define a GET route in axum?
   .route("/path", get(handler_function))

3. How do you extract a path parameter like `/user/:id` in axum?
   Path(id): Path<Type>

4. What does `Json(response)` do in a handler function?
   wraps the value as a JSON response with the correct content type

5. What crate provides the async runtime for axum?
   tokio

---

**Check your answers against the lesson after you finish.**
