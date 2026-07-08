# DAY21: Checkpoint

1. What HTTP status code indicates a successful WebSocket upgrade?

   <details>
   <summary>Answer</summary>
   **101 Switching Protocols**
   </details>

2. How does a WebSocket connection start?

   <details>
   <summary>Answer</summary>
   As a regular HTTP request with an `Upgrade: websocket` header. The server responds with `101`, and the protocol switches.
   </details>

3. What's the key difference between HTTP and WebSocket communication?

   <details>
   <summary>Answer</summary>
   HTTP is **request-response** (client initiates). WebSocket is **bidirectional** — either side can send data at any time.
   </details>

4. When should you use WebSocket over HTTP?

   <details>
   <summary>Answer</summary>
   For **real-time, low-latency, bidirectional** communication: chat, live updates, games, collaborative editing.
   </details>

5. True or false: WebSocket replaces HTTP for all use cases.

   <details>
   <summary>Answer</summary>
   **False** — WebSocket is a complement to HTTP, not a replacement. Use HTTP for standard API calls and WebSocket for real-time features.
   </details>
