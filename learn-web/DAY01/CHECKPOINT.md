# DAY01: Checkpoint

**Closed book.** No looking back at the lesson. Answer in your own words.

1. In the postal service analogy, what part of an HTTP conversation is the "letter you send"?

   <details>
   <summary>Answer</summary>
   The **request** (client → server).
   </details>

2. What does `curl -v` do that plain `curl` doesn't?

   <details>
   <summary>Answer</summary>
   It shows the **verbose** output, including the request line, headers, and response status — not just the body.
   </details>

3. You run `curl https://example.com` and see `< 301 Moved Permanently`. What changed?

   <details>
   <summary>Answer</summary>
   The response status — instead of `200 OK`, the server returned a redirect. curl is still showing the conversation; the server just decided to send a different response.
   </details>

4. What is the difference between a request header and a response header?

   <details>
   <summary>Answer</summary>
   Request headers are sent **from client to server** (like `User-Agent`, `Accept`). Response headers are sent **from server to client** (like `Content-Type`, `Content-Length`).
   </details>

5. A URL has `?search=golang&page=1` at the end. What is this part called?

   <details>
   <summary>Answer</summary>
   The **query string** (or query parameters).
   </details>

---

**Check your answers against the lesson after you finish.**
