# DAY22: Drill — "Hit the rate limit"

Trigger rate limits and build your own rate limiter.

## Part 1 — Observe GitHub's rate limit

```bash
# Check your current rate limit status
curl -I https://api.github.com/users/scott
# Look for: X-RateLimit-Remaining

# Without auth, GitHub limits to 60/hour
# Hit it multiple times and watch the remaining count drop
for i in {1..5}; do
  curl -s -o /dev/null -w "%{http_code} (remaining: %{header{X-RateLimit-Remaining}})\n" \
    https://api.github.com/users/scott
  sleep 0.5
done
```

## Part 2 — Trigger a 429

```bash
# httpbin has a rate limit endpoint
for i in {1..20}; do
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" "https://httpbin.org/status/429")
  echo "Request $i: $STATUS"
done
```

## Part 3 — Build a rate-limited server

Create `server.go` integrating the rate limiter from the lesson:

```go
package main

import (
    "encoding/json"
    "fmt"
    "net/http"
    "sync"
    "time"
)

type Visitor struct {
    count    int
    lastSeen time.Time
}

type RateLimiter struct {
    mu       sync.Mutex
    visitors map[string]*Visitor
    rate     int
    window   time.Duration
}

func NewRateLimiter(rate int, window time.Duration) *RateLimiter {
    rl := &RateLimiter{
        visitors: make(map[string]*Visitor),
        rate:     rate,
        window:   window,
    }
    go func() {
        for {
            time.Sleep(time.Minute)
            rl.mu.Lock()
            for ip, v := range rl.visitors {
                if time.Since(v.lastSeen) > rl.window*2 {
                    delete(rl.visitors, ip)
                }
            }
            rl.mu.Unlock()
        }
    }()
    return rl
}

func (rl *RateLimiter) Allow(ip string) bool {
    rl.mu.Lock()
    defer rl.mu.Unlock()
    now := time.Now()
    v, exists := rl.visitors[ip]
    if !exists {
        rl.visitors[ip] = &Visitor{count: 1, lastSeen: now}
        return true
    }
    if now.Sub(v.lastSeen) > rl.window {
        v.count = 1
        v.lastSeen = now
        return true
    }
    v.lastSeen = now
    if v.count >= rl.rate {
        return false
    }
    v.count++
    return true
}

var rl = NewRateLimiter(5, 10*time.Second) // 5 requests per 10 seconds

func handler(w http.ResponseWriter, r *http.Request) {
    ip := r.RemoteAddr
    if !rl.Allow(ip) {
        w.Header().Set("Retry-After", "10")
        http.Error(w, `{"error":"rate limited"}`, http.StatusTooManyRequests)
        return
    }
    json.NewEncoder(w).Encode(map[string]interface{}{
        "message": "ok",
        "remaining": "check headers",
    })
}

func main() {
    http.HandleFunc("/api", handler)
    fmt.Println("Rate-limited server on :8080 (5 req/10s per IP)")
    http.ListenAndServe(":8080", nil)
}
```

## Part 4 — Test your rate limiter

```bash
# Start the server
go run server.go &

# Hit it faster than allowed
for i in {1..10}; do
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/api)
  echo "Request $i: $STATUS"
  sleep 0.5
done
# You should see 200 for the first 5, then 429

kill %1
```

## Part 5 — Add rate limit headers

Modify the server to include `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset` headers in the response.

## Deliverables

1. Your remaining GitHub rate limit from Part 1
2. The output from Part 4 (200s then 429s)
3. Your server code with rate limit headers

## Hints

<details>
<summary>Click for hints</summary>

- GitHub's unauthenticated rate limit is 60/hr; authenticated is 5000/hr
- `http.StatusTooManyRequests` = 429
- `Retry-After` header tells the client how many seconds to wait
- The cleanup goroutine prevents memory leaks from old visitors
- Rate limit by IP is a simple approach — real systems also rate limit by API key, user ID, etc.
</details>
