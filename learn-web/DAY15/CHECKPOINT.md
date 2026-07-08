# DAY15: Checkpoint

1. Why should you centralise HTTP calls in a dedicated client package?

   <details>
   <summary>Answer</summary>
   **Consistency** — auth headers, timeouts, error handling, and base URL are all in one place.
   </details>

2. What does `defer resp.Body.Close()` do in the `doRequest` method?

   <details>
   <summary>Answer</summary>
   Closes the response body when the function returns, **preventing resource leaks** and allowing connection reuse.
   </details>

3. Why does the client use `json.NewDecoder` instead of `io.ReadAll` + `json.Unmarshal`?

   <details>
   <summary>Answer</summary>
   `json.NewDecoder` **streams** JSON from the response body — more efficient for large responses and uses less memory.
   </details>

4. What happens in the `doRequest` method when the server returns a 404?

   <details>
   <summary>Answer</summary>
   The response body is read and closed, then an **error** is returned with the status code and body content.
   </details>

5. Why does the constructor `NewClient` take the token as a parameter instead of reading the env var inside?

   <details>
   <summary>Answer</summary>
   **Separation of concerns** — the client shouldn't know about environment variables. It just needs the token to function.
   </details>
