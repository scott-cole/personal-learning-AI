# DAY26: Drill — "Add Rate Limiting Middleware"

Add a token-bucket rate limiter as middleware to the URL shortener.

## Structure

```
DAY26/
├── main.go              → rate limited handlers
├── storage/
│   └── memory.go
├── service/
│   └── service.go
└── middleware/
    └── ratelimit.go     → RateLimiter + middleware
```

## middleware/ratelimit.go

```go
package middleware

import (
    "net/http"
    "sync"
    "time"
)

type RateLimiter struct {
    mu       sync.Mutex
    tokens   int
    max      int
    interval time.Duration
}

func NewRateLimiter(max int, interval time.Duration) *RateLimiter {
    rl := &RateLimiter{
        tokens:   max,
        max:      max,
        interval: interval,
    }
    go func() {
        ticker := time.NewTicker(interval)
        for range ticker.C {
            rl.mu.Lock()
            if rl.tokens < rl.max {
                rl.tokens++
            }
            rl.mu.Unlock()
        }
    }()
    return rl
}

func (rl *RateLimiter) Allow() bool {
    rl.mu.Lock()
    defer rl.mu.Unlock()
    if rl.tokens > 0 {
        rl.tokens--
        return true
    }
    return false
}

func RateLimit(rl *RateLimiter) func(http.Handler) http.Handler {
    return func(next http.Handler) http.Handler {
        return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
            if !rl.Allow() {
                w.Header().Set("Retry-After", "1")
                http.Error(w, "too many requests", http.StatusTooManyRequests)
                return
            }
            next.ServeHTTP(w, r)
        })
    }
}
```

## main.go

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "time"
    "learn-go/DAY26/middleware"
    "learn-go/DAY26/service"
    "learn-go/DAY26/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    // 5 requests per second
    limiter := middleware.NewRateLimiter(5, 200*time.Millisecond)

    mux := http.NewServeMux()

    mux.Handle("POST /shorten", middleware.RateLimit(limiter)(http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        var req struct {
            URL string `json:"url"`
        }
        json.NewDecoder(r.Body).Decode(&req)
        result, err := svc.Shorten(req.URL)
        if err != nil {
            http.Error(w, err.Error(), http.StatusBadRequest)
            return
        }
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusCreated)
        json.NewEncoder(w).Encode(result)
    })))

    mux.HandleFunc("GET /{code}", func(w http.ResponseWriter, r *http.Request) {
        code := r.PathValue("code")
        url, err := svc.Resolve(code)
        if err != nil {
            http.Error(w, "not found", http.StatusNotFound)
            return
        }
        http.Redirect(w, r, url.OriginalURL, http.StatusFound)
    })

    fmt.Println("Server starting on :8080")
    log.Fatal(http.ListenAndServe(":8080", mux))
}
```

## Test it

```bash
cd ~/Dev/learn-go/DAY26
go mod init learn-go/DAY26
go run main.go
```

In another terminal, send 10 requests fast:

```bash
for i in $(seq 1 10); do
  curl -s -w "\nHTTP %{http_code}\n" -X POST http://localhost:8080/shorten \
    -H "Content-Type: application/json" \
    -d '{"url":"https://example.com"}' &
done
wait
```

You should see some `201 Created` and some `429 Too Many Requests`.

## Run it

```bash
cd ~/Dev/learn-go/DAY26
go run main.go
```
