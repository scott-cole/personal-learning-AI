# DAY13: Checkpoint

**Closed book.**

1. What is a Go middleware function's signature? (What does it take and return?)

2. In the logging middleware, where does `next.ServeHTTP(w, r)` go? Before logging or after? Why?

3. What does this middleware do? What's the bug?
   ```go
   func auth(next http.Handler) http.Handler {
       return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
           token := r.Header.Get("Authorization")
           if token == "" {
               http.Error(w, "unauthorized", http.StatusUnauthorized)
           }
           next.ServeHTTP(w, r)
       })
   }
   ```

4. Why do you wrap handlers in middleware instead of putting the logic directly in each handler?

5. What order do middleware run if you apply them like: `m1(m2(handler))`?
