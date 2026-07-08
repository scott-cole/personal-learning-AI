# DAY10: Checkpoint

1. You want to get user with ID 42. Should you use a path parameter or a query parameter?

   <details>
   <summary>Answer</summary>
   **Path parameter** — `/users/42`. It identifies a specific resource.
   </details>

2. What's the purpose of URL encoding (percent-encoding)?

   <details>
   <summary>Answer</summary>
   To make special characters (spaces, `&`, `?`, `#`) safe for use in URLs.
   </details>

3. How do you pass multiple values for the same query parameter?

   <details>
   <summary>Answer</summary>
   Repeat the key: `?id=1&id=2&id=3`
   </details>

4. What does `?` do in a URL?

   <details>
   <summary>Answer</summary>
   It **separates the path from the query string**.
   </details>

5. A URL has `#section` at the end. Does the server receive this?

   <details>
   <summary>Answer</summary>
   No — the **fragment** (`#section`) is used by the browser and never sent to the server.
   </details>
