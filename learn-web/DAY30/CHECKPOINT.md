# DAY30: Checkpoint

1. What status code should the short URL creation endpoint return?

   <details>
   <summary>Answer</summary>
   **201 Created** — a new resource was created.
   </details>

2. Why does the redirect endpoint return a 302 instead of serving the content directly?

   <details>
   <summary>Answer</summary>
   The URL shortener's job is to **redirect** to the original URL, not to serve it. The browser handles the redirect automatically.
   </details>

3. What header tells the browser where to go in a redirect?

   <details>
   <summary>Answer</summary>
   The **`Location`** header contains the target URL.
   </details>

4. In the rate limiter, what determines whether a request is allowed?

   <details>
   <summary>Answer</summary>
   The number of requests from that IP within the time window. If it exceeds the limit, the request is denied with **429**.
   </details>

5. What happens if you don't handle SIGTERM in your Go server?

   <details>
   <summary>Answer</summary>
   The process **terminates immediately** — in-flight requests are cut off, database connections are dropped, and the server doesn't clean up.
   </details>
