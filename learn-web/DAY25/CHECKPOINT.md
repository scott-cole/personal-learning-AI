# DAY25: Checkpoint

1. What is a reverse proxy?

   <details>
   <summary>Answer</summary>
   A server that sits **in front of backend servers**, forwarding client requests and handling tasks like TLS termination, load balancing, and caching.
   </details>

2. When a request goes through a reverse proxy, where does the backend see the client's real IP?

   <details>
   <summary>Answer</summary>
   In the **`X-Forwarded-For`** header (added by the proxy). `r.RemoteAddr` will be the proxy's IP.
   </details>

3. What's one reason to use a reverse proxy instead of exposing your app directly?

   <details>
   <summary>Answer</summary>
   TLS termination, rate limiting, load balancing, caching, or hiding backend infrastructure.
   </details>

4. What does `proxy_pass http://localhost:3000;` do in nginx?

   <details>
   <summary>Answer</summary>
   Tells nginx to **forward incoming requests** to the backend server running on `localhost:3000`.
   </details>

5. True or false: A reverse proxy is required for every web application.

   <details>
   <summary>Answer</summary>
   **False** — simple applications or personal projects can run directly. Proxies are needed for production with multiple backends, TLS, or load balancing.
   </details>
