# DAY29: Checkpoint

1. What status code should the short URL redirect endpoint return?

   <details>
   <summary>Answer</summary>
   **302 Found** (temporary redirect) — allows updating the destination URL later. 301 would be cached permanently by browsers.
   </details>

2. How would you generate a unique short code?

   <details>
   <summary>Answer</summary>
   Options: random base62 string (e.g., 7 chars from `[a-zA-Z0-9]`), hash of the URL (MD5/SHA1 truncated), or auto-incrementing counter encoded in base62.
   </details>

3. What headers should the rate limiter include in its response?

   <details>
   <summary>Answer</summary>
   `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`, and `Retry-After` when rate limited.
   </details>

4. Why would you use 302 instead of 301 for the redirect?

   <details>
   <summary>Answer</summary>
   **301 is cached permanently** by browsers. If you want to change the destination URL later, users with cached 301 would still go to the old URL. 302 is not cached and always checked.
   </details>

5. What's the first thing you should validate when someone submits a URL to shorten?

   <details>
   <summary>Answer</summary>
   That the URL is **valid** (proper format, valid scheme like http/https, not a malicious scheme like `javascript:`).
   </details>
