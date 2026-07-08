# DAY25: Checkpoint

**Closed book.** No looking back.

1. What HTTP method is used to create a resource?
   POST

2. What does `State(state): State<AppState>` do in an axum handler?
   injects shared application state into the handler

3. What does `StatusCode::NOT_FOUND` represent?
   404 — the requested resource was not found

4. How do you return both a status code and JSON in axum?
   (StatusCode::CREATED, Json(data))

5. What's the difference between `get(list_books)` and `.post(create_book)` chained on the same route?
   they handle different HTTP methods on the same path

---

**Check your answers against the lesson after you finish.**
