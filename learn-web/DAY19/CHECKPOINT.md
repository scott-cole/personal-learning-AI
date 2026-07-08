# DAY19: Checkpoint

1. What does DNS do?

   <details>
   <summary>Answer</summary>
   Converts **human-readable hostnames** (like `example.com`) to **machine-readable IP addresses** (like `93.184.216.34`).
   </details>

2. What's the difference between an A record and a CNAME record?

   <details>
   <summary>Answer</summary>
   An **A record** maps a hostname directly to an IP address. A **CNAME** maps a hostname to **another hostname** (an alias).
   </details>

3. What does TTL mean in a DNS record?

   <details>
   <summary>Answer</summary>
   **Time To Live** — how many seconds a resolver can cache the record before checking for a new value.
   </details>

4. You change your website's IP address. Why do some users still see the old IP?

   <details>
   <summary>Answer</summary>
   Their DNS resolver still has the **old A record cached**. They'll get the new IP once the TTL expires and the resolver refreshes.
   </details>

5. How can you query a specific DNS resolver (like Google's 8.8.8.8) instead of your default?

   <details>
   <summary>Answer</summary>
   Use `dig @8.8.8.8 example.com` or `nslookup example.com 8.8.8.8`.
   </details>
