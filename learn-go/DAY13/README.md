# DAY13: Middleware — "The Security Guard"

---

## The anecdote: "Middleware is a security guard"

Before you enter a club, the security guard:
1. Checks your ID (log the request)
2. Stamps your hand (add a request ID)
3. Maybe stops you at the door (reject if rate limited)
4. Lets you in (pass to the handler)

```
Request → [Middleware 1] → [Middleware 2] → [Handler]
               ↑                ↑
          Logging           Rate limiting
```

In Go, middleware is a function that wraps a handler:

```go
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        log.Printf("%s %s", r.Method, r.URL.Path)
        next.ServeHTTP(w, r)  // pass to the next handler
    })
}
```

---

## How middleware works

```go
// A middleware takes a handler and returns a handler
type Middleware func(http.Handler) http.Handler
```

**Before** the handler runs → setup (logging, auth)
**After** `next.ServeHTTP(w, r)` → teardown (cleanup, timing)

---

## Chaining middleware

```go
func applyMiddleware(handler http.Handler, middlewares ...Middleware) http.Handler {
    for i := len(middlewares) - 1; i >= 0; i-- {
        handler = middlewares[i](handler)
    }
    return handler
}

// Usage
handler := applyMiddleware(myHandler, loggingMiddleware, authMiddleware)
```

Or with `http.ServeMux`:

```go
mux := http.NewServeMux()
mux.Handle("GET /", loggingMiddleware(http.HandlerFunc(helloHandler)))
```

---

## Common middleware examples

```go
// Logging
func logging(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        log.Printf("%s %s %s", r.Method, r.URL.Path, time.Since(start))
    })
}

// Request ID
func requestID(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        id := fmt.Sprintf("req-%d", time.Now().UnixNano())
        r.Header.Set("X-Request-ID", id)
        w.Header().Set("X-Request-ID", id)
        next.ServeHTTP(w, r)
    })
}

// Recovery (catch panics)
func recovery(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        defer func() {
            if err := recover(); err != nil {
                http.Error(w, "internal server error", http.StatusInternalServerError)
            }
        }()
        next.ServeHTTP(w, r)
    })
}
```

---

## Senior mindset: "Middleware is cross-cutting concern"

Things that apply to **every** request belong in middleware:
- Logging
- Authentication
- Rate limiting
- Request ID
- CORS headers
- Recovery from panics

Things that are **specific to one endpoint** belong in the handler.

Open DAY13/DRILL.md.
