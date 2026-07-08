# DAY26: Checkpoint

1. What algorithm does round-robin load balancing use to distribute requests?

   <details>
   <summary>Answer</summary>
   Cycles through servers in order: Server A, B, C, then back to A.
   </details>

2. Why are sticky sessions considered a design smell?

   <details>
   <summary>Answer</summary>
   They mean the backend has **local state** — which prevents truly horizontal scaling. A better approach is a shared session store (Redis).
   </details>

3. What happens when a backend server fails and the load balancer doesn't have health checks?

   <details>
   <summary>Answer</summary>
   The load balancer still sends requests to the failed server, causing **connection errors** for some users.
   </details>

4. What's the difference between passive and active health checks?

   <details>
   <summary>Answer</summary>
   **Passive**: Observes real traffic (marks backend down after N failures). **Active**: Periodically pings a health endpoint.
   </details>

5. In the least-connections algorithm, which server does the load balancer send the next request to?

   <details>
   <summary>Answer</summary>
   The server with the **fewest active connections** — better for requests with different processing times.
   </details>
