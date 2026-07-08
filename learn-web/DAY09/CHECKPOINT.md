# DAY09: Checkpoint

1. Why should URLs use nouns instead of verbs?

   <details>
   <summary>Answer</summary>
   The HTTP method **is** the verb (GET, POST, etc.). Using verbs in URLs is redundant and inconsistent.
   </details>

2. What's the proper HTTP status code for a successful POST that creates a resource?

   <details>
   <summary>Answer</summary>
   **201 Created**
   </details>

3. You have a resource at `/users/42/orders`. What does this URL structure imply about the relationship?

   <details>
   <summary>Answer</summary>
   Orders **belong to** user 42 (a nested/hierarchical relationship).
   </details>

4. What's wrong with this API design: `GET /deleteUser?id=42`?

   <details>
   <summary>Answer</summary>
   It uses a verb in the URL (`deleteUser`) and a query parameter for the ID. Should be `DELETE /users/42`.
   </details>

5. What should an error response in a REST API look like?

   <details>
   <summary>Answer</summary>
   A consistent JSON structure with error code, message, and optional details — same format for every error.
   </details>
