# DAY14: Checkpoint

1. What's the Same-Origin Policy?

   <details>
   <summary>Answer</summary>
   A browser security rule that blocks a web page from making requests to a **different origin** (different scheme, host, or port).
   </details>

2. Does CORS protect your API from server-side abuse?

   <details>
   <summary>Answer</summary>
   No — CORS is only enforced by **browsers**. Curl, Postman, backend servers can bypass it.
   </details>

3. What triggers a CORS preflight (OPTIONS) request?

   <details>
   <summary>Answer</summary>
   Requests with methods other than GET/HEAD/POST, custom headers, or non-simple content types (like `application/json`).
   </details>

4. What does `Access-Control-Allow-Origin: *` mean?

   <details>
   <summary>Answer</summary>
   Any origin can access this resource (but can't include credentials like cookies).
   </details>

5. You make a DELETE request from your frontend and get a CORS error. What's the first thing to check?

   <details>
   <summary>Answer</summary>
   Whether the server includes `Access-Control-Allow-Methods: DELETE` in its preflight response (or handles OPTIONS requests).
   </details>
