# DAY02: Checkpoint

1. Which HTTP method should you use to **read** a resource without changing anything?

   <details>
   <summary>Answer</summary>
   GET
   </details>

2. You send a POST request twice. The server creates two records. Why is this expected?

   <details>
   <summary>Answer</summary>
   POST is **not idempotent** — each request creates a new resource.
   </details>

3. What's the difference between PUT and PATCH?

   <details>
   <summary>Answer</summary>
   PUT **replaces** the entire resource. PATCH **updates only the fields you send**.
   </details>

4. Which method should return `201 Created` on success?

   <details>
   <summary>Answer</summary>
   POST
   </details>

5. True or false: GET requests can have a body.

   <details>
   <summary>Answer</summary>
   False — GET requests should not have a body. The RFC doesn't forbid it, but servers typically ignore it.
   </details>
