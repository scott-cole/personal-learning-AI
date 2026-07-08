# DAY23: Checkpoint

**Closed book.** No looking back.

1. What do the three type parameters of `Request<Params, ResBody, ReqBody>` represent?

2. How do you return a 404 status with Express?

3. What does `app.use(express.json())` do?

4. How is `Router` different from using `app` directly?

5. What is middleware in Express?

---

<details>
<summary>Answers</summary>

1. `Params` — URL parameters (e.g., `:id`), `ResBody` — the response body type, `ReqBody` — the request body type.
2. `res.status(404).json({ error: "Not found" })` or `.send()` instead of `.json()`.
3. It adds middleware that parses incoming JSON request bodies into `req.body` (JavaScript object).
4. `Router` creates a modular route group. You define routes on it and mount it with `app.use("/prefix", router)`. It's cleaner than putting all routes on `app`.
5. Functions that run in the request-response cycle before your route handlers. They can log, authenticate, parse bodies, etc.
</details>
