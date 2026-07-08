# DAY14: Week 2 Checkpoint

**Cumulative test of Week 2.** Answer without looking back.

1. Write an interface `Cache` with `Get(key string) (string, error)` and `Set(key, value string) error`.

2. What does accepting an interface as a parameter enable that accepting a concrete type doesn't?

3. Order these layers from bottom to top: Service, Repository, Handler, Middleware.

4. What's the difference between `json.Marshal` and `json.NewEncoder`?

5. Write the Go code to read JSON from an HTTP request body into a struct called `CreateRequest`.

6. What does this middleware do?
   ```go
   func timing(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           start := time.Now()
           next.ServeHTTP(w, r)
           log.Printf("%s took %s", r.URL.Path, time.Since(start))
       })
   }
   ```

7. Why do we use dependency injection (pass repo to service) instead of having the service create its own repo?

8. What HTTP status code would you use for:
   - Successful creation → ?
   - Not found → ?
   - Bad request → ?
   - Redirect → ?

9. What does `r.PathValue("code")` do?

10. What's the problem with this handler?
    ```go
    func handler(w http.ResponseWriter, r *http.Request) {
        json.NewEncoder(w).Encode(result)
    }
    ```
