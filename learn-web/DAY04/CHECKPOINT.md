# DAY04: Checkpoint

1. What is the purpose of the `Content-Type` header?

   <details>
   <summary>Answer</summary>
   It tells the receiver what **format** the body is in (JSON, HTML, XML, etc.).
   </details>

2. You want to tell a server you prefer French. What header do you send?

   <details>
   <summary>Answer</summary>
   `Accept-Language: fr`
   </details>

3. A server sends `Cache-Control: no-cache` in its response. What is it telling the client?

   <details>
   <summary>Answer</summary>
   Don't cache this response (or revalidate before using a cached copy).
   </details>

4. What does the `Authorization` header typically contain?

   <details>
   <summary>Answer</summary>
   Credentials, usually `Basic <base64-encoded-credentials>` or `Bearer <token>`.
   </details>

5. True or false: `content-type` and `Content-Type` are different headers.

   <details>
   <summary>Answer</summary>
   False — HTTP headers are **case-insensitive**.
   </details>
