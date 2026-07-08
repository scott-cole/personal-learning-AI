# DAY05: Checkpoint

1. You run `curl -d "name=Scott" https://httpbin.org/post`. What `Content-Type` does curl use by default?

   <details>
   <summary>Answer</summary>
   `application/x-www-form-urlencoded`
   </details>

2. What does the `@` symbol mean in `curl -d @data.json`?

   <details>
   <summary>Answer</summary>
   Read the body from the file `data.json` instead of using the string literal.
   </details>

3. Why must you always `defer resp.Body.Close()` in Go?

   <details>
   <summary>Answer</summary>
   To prevent resource leaks — response bodies are streamed and must be closed to reuse the connection.
   </details>

4. What's the difference between `-d` and `-F` in curl?

   <details>
   <summary>Answer</summary>
   `-d` sends URL-encoded form data (or raw text). `-F` sends `multipart/form-data` (for file uploads).
   </details>

5. You want to send raw JSON in a POST request. What two flags do you need?

   <details>
   <summary>Answer</summary>
   `-d '{"key":"value"}'` and `-H "Content-Type: application/json"`
   </details>
