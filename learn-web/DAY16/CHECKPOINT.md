# DAY16: Checkpoint

1. What does the `HttpOnly` cookie flag do?

   <details>
   <summary>Answer</summary>
   It prevents JavaScript (via `document.cookie`) from reading the cookie, protecting against XSS attacks.
   </details>

2. How does the browser know to send a cookie back to the server?

   <details>
   <summary>Answer</summary>
   The browser automatically stores cookies from `Set-Cookie` headers and sends them via the `Cookie` header on subsequent requests to the same domain.
   </details>

3. What cookie flag prevents cookies from being sent over unencrypted HTTP?

   <details>
   <summary>Answer</summary>
   **`Secure`** — the cookie is only sent over HTTPS connections.
   </details>

4. In curl, what flag writes cookies to a file and what flag reads them?

   <details>
   <summary>Answer</summary>
   `-c` (or `--cookie-jar`) writes cookies. `-b` (or `--cookie`) reads cookies.
   </details>

5. What's the difference between `Expires` and `Max-Age` in a cookie?

   <details>
   <summary>Answer</summary>
   `Expires` is an absolute date (HTTP date format). `Max-Age` is a **relative** number of seconds from now. Max-Age takes precedence if both are set.
   </details>
