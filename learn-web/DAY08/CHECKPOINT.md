# DAY08: Checkpoint

1. What's wrong with `http.Get("https://example.com")` in production code?

   <details>
   <summary>Answer</summary>
   It uses the **default client which has no timeout** — if the server hangs, your program hangs forever.
   </details>

2. What does `defer resp.Body.Close()` do? Why must it be called?

   <details>
   <summary>Answer</summary>
   It closes the response body stream when the function returns, **preventing resource leaks** and allowing connection reuse.
   </details>

3. You call `client.Do(req)` and get an error. Should you still call `resp.Body.Close()`?

   <details>
   <summary>Answer</summary>
   No — if `Do` returns an error, `resp` is **nil**.
   </details>

4. How do you send a JSON body in a Go HTTP request?

   <details>
   <summary>Answer</summary>
   Marshal the struct with `json.Marshal`, wrap in `bytes.NewReader`, set `Content-Type: application/json`.
   </details>

5. What method do you call to add a header to a `http.Request`?

   <details>
   <summary>Answer</summary>
   `req.Header.Set("Key", "Value")`
   </details>
