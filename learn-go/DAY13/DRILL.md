# DAY13: Drill — "Add Middleware to the URL Shortener"

Add logging and request ID middleware to your URL shortener.

## Structure

Same as DAY12 — `main.go` with handlers. Just add middleware functions.

## Requirements

Add these middleware functions to `main.go`:

```go
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        log.Printf("%s %s %s", r.Method, r.URL.Path, time.Since(start))
    })
}

func requestIDMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        id := fmt.Sprintf("%d", time.Now().UnixNano())
        w.Header().Set("X-Request-ID", id)
        next.ServeHTTP(w, r)
    })
}
```

Then wrap your handlers:

```go
mux.Handle("POST /shorten", loggingMiddleware(requestIDMiddleware(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    // ... handler code
}))))

mux.Handle("GET /{code}", loggingMiddleware(requestIDMiddleware(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    // ... handler code
}))))
```

## Test it

```bash
cd ~/Dev/learn-go/DAY13
go run main.go
```

In another terminal:

```bash
curl -v -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'
```

Look for the `X-Request-ID` header in the response and the log line in the server output.

### Expected log output (server terminal)

```
2026/06/29 12:00:00 POST /shorten 1.234ms
2026/06/29 12:00:05 GET /aB3xK9mQ 0.567ms
```

## Run it

```bash
cd ~/Dev/learn-go/DAY13
go run main.go
```
