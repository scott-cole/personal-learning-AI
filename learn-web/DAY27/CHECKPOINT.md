# DAY27: Checkpoint

1. What does PoP stand for in CDN terminology?

   <details>
   <summary>Answer</summary>
   **Point of Presence** — a CDN server location geographically close to users.
   </details>

2. What header tells a CDN how long to cache a response?

   <details>
   <summary>Answer</summary>
   `s-maxage` in the `Cache-Control` header (e.g., `Cache-Control: public, s-maxage=86400`).
   </details>

3. What's the difference between a "cache hit" and a "cache miss" on a CDN?

   <details>
   <summary>Answer</summary>
   **Cache hit**: The CDN already has the content and serves it immediately (fast). **Cache miss**: The CDN must fetch from the origin server (slower for the first request).
   </details>

4. Why should you use cache busting (hash in filename) instead of CDN purge?

   <details>
   <summary>Answer</summary>
   Purge is immediate but has rate limits and is complex. Cache busting is **automatic** — the new filename creates a new cache entry, and old entries expire naturally.
   </details>

5. A user in Australia visits your site. Without CDN, the request goes to New York. With CDN, where does it go?

   <details>
   <summary>Answer</summary>
   To the **nearest CDN PoP** (e.g., Sydney or Melbourne). The CDN fetches from origin on first request, then serves from the local PoP for subsequent requests.
   </details>
