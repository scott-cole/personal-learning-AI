# DAY24: Checkpoint

1. What does TTFB (Time To First Byte) measure?

   <details>
   <summary>Answer</summary>
   The time from when the request is sent to when the **first byte of the response** is received — essentially the server's processing time.
   </details>

2. What's the main advantage of HTTPie over curl for ad-hoc API testing?

   <details>
   <summary>Answer</summary>
   **Readability** — syntax-highlighted, formatted JSON output, and simpler syntax for sending JSON data.
   </details>

3. In DevTools Network tab, what does the Timing tab tell you that Headers doesn't?

   <details>
   <summary>Answer</summary>
   The **breakdown of request phases** — DNS lookup, TCP connect, TLS handshake, TTFB, and content download time.
   </details>

4. Why should you check the "Disable cache" checkbox during development?

   <details>
   <summary>Answer</summary>
   To ensure you always get **fresh responses** instead of cached ones — your code changes take effect immediately.
   </details>

5. You open DevTools and see a request with status "(canceled)". What does this mean?

   <details>
   <summary>Answer</summary>
   The request was **cancelled** before completing — usually because the page navigated away, JavaScript aborted it, or another higher-priority request took its place.
   </details>
