# DAY11: Checkpoint

1. Why put version in the URL path instead of using a header?

   <details>
   <summary>Answer</summary>
   URL path versioning is **visible, simple to route, and easy to test** — you can just change the URL.
   </details>

2. What's the main disadvantage of page-based pagination?

   <details>
   <summary>Answer</summary>
   Pages are **unstable** — if items are inserted or deleted, the same page can show different items between requests.
   </details>

3. How does the GitHub API tell you about the next page of results?

   <details>
   <summary>Answer</summary>
   Via the **`Link` header** which contains `next`, `last`, `first`, and `prev` rel links.
   </details>

4. You're filtering a collection by status and category. What would the URL look like?

   <details>
   <summary>Answer</summary>
   `?status=active&category=books`
   </details>

5. Why should every collection endpoint have pagination, even small ones?

   <details>
   <summary>Answer</summary>
   Data grows over time. A 50-row response today could be 50,000 rows next year. Pagination prevents accidental overload.
   </details>
