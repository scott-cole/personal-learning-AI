# DAY18: Checkpoint

1. What does `Cache-Control: max-age=3600` mean?

   <details>
   <summary>Answer</summary>
   The response can be cached for **3600 seconds (1 hour)** before the client must check with the server.
   </details>

2. What's the difference between `no-cache` and `no-store`?

   <details>
   <summary>Answer</summary>
   `no-cache` means "must revalidate before using" (still cache it, just check first). `no-store` means **don't cache at all**.
   </details>

3. How does an ETag reduce bandwidth?

   <details>
   <summary>Answer</summary>
   The client sends `If-None-Match` with the previous ETag. If the content hasn't changed, the server returns **304 Not Modified** with no body — saving bandwidth.
   </details>

4. You updated your website's CSS but users see the old version. What happened?

   <details>
   <summary>Answer</summary>
   The old CSS was cached by the browser. You need **cache busting** — change the filename or add a version query parameter.
   </details>

5. What HTTP status code does the server return when the client's cached copy is still valid?

   <details>
   <summary>Answer</summary>
   **304 Not Modified**
   </details>
