# DAY22: Checkpoint

1. What HTTP status code indicates the client is being rate limited?

   <details>
   <summary>Answer</summary>
   **429 Too Many Requests**
   </details>

2. What header does a rate-limited API use to tell the client how long to wait?

   <details>
   <summary>Answer</summary>
   `Retry-After` (in seconds or HTTP date).
   </details>

3. Explain the token bucket algorithm.

   <details>
   <summary>Answer</summary>
   A bucket holds N tokens. Each request consumes 1 token. Tokens are refilled at a fixed rate (e.g., 1 per second). If the bucket is empty, the request is denied.
   </details>

4. Why should rate limiting happen at the reverse proxy level instead of the app level?

   <details>
   <summary>Answer</summary>
   To **protect your infrastructure** — if a malicious request reaches your app server, it already used CPU and resources. Block at the edge to prevent resource consumption.
   </details>

5. What does `X-RateLimit-Reset` contain?

   <details>
   <summary>Answer</summary>
   A **Unix timestamp** indicating when the current rate limit window resets.
   </details>
