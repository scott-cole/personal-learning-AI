# DAY03: Checkpoint

1. You visit `http://example.com` and your browser automatically changes it to `https://example.com`. What status code caused that?

   <details>
   <summary>Answer</summary>
   **301 Moved Permanently** — the server told the browser "this URL is now HTTPS."
   </details>

2. A user sends bad input that fails validation. What status code should the API return?

   <details>
   <summary>Answer</summary>
   **400 Bad Request** or **422 Unprocessable Entity**
   </details>

3. What's the difference between 401 Unauthorized and 403 Forbidden?

   <details>
   <summary>Answer</summary>
   **401** means "you haven't authenticated" (log in). **403** means "you're authenticated but not allowed" (no permission).
   </details>

4. Your database crashes mid-request. What status code should the server return?

   <details>
   <summary>Answer</summary>
   **500 Internal Server Error**
   </details>

5. Your server is down for maintenance. What status code should it return?

   <details>
   <summary>Answer</summary>
   **503 Service Unavailable**
   </details>
