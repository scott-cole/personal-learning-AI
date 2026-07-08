# DAY06: Checkpoint

1. What problem did HTTP/2's multiplexing solve?

   <details>
   <summary>Answer</summary>
   **Head-of-line blocking** — HTTP/1.1 could only send one request at a time per connection.
   </details>

2. What transport protocol does HTTP/3 use instead of TCP?

   <details>
   <summary>Answer</summary>
   **QUIC**, which runs on **UDP**.
   </details>

3. True or false: HTTP/3 requires encryption.

   <details>
   <summary>Answer</summary>
   True — HTTP/3 (QUIC) has encryption built in.
   </details>

4. You're building a web app. Do you need to write code differently for HTTP/2 vs HTTP/3?

   <details>
   <summary>Answer</summary>
   No — your HTTP client library handles protocol negotiation. Write normal HTTP code.
   </details>

5. What does 0-RTT mean in HTTP/3?

   <details>
   <summary>Answer</summary>
   **Zero round trips** for connection setup — data can be sent immediately on the first packet for previously visited sites.
   </details>
