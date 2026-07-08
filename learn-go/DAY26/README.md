# DAY26: Rate Limiting — "Protecting Your Service"

---

## The anecdote: "A bouncer at the door"

A club holds 100 people. If 500 try to enter at once, chaos.

The bouncer lets people in at a steady rate: 1 person per second. If you try to push past, you're sent to the back of the line.

Rate limiting is the bouncer for your server.

```
Without rate limiting:     With rate limiting:
1000 req/s → server dies   10 req/s → 10 served, 990 "too many"
```

---

## Token bucket algorithm

The simplest rate limiter: a bucket of tokens that refills over time.

```go
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
    // Refill tokens every interval
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
```

---

## Rate limiting middleware

```go
func rateLimitMiddleware(rl *RateLimiter) func(http.Handler) http.Handler {
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

Status code `429 Too Many Requests` is the standard response.

---

## Per-IP rate limiting

```go
type IPRateLimiter struct {
    mu       sync.Mutex
    limiters map[string]*RateLimiter
    max      int
    interval time.Duration
}

func (irl *IPRateLimiter) GetLimiter(ip string) *RateLimiter {
    irl.mu.Lock()
    defer irl.mu.Unlock()
    if _, ok := irl.limiters[ip]; !ok {
        irl.limiters[ip] = NewRateLimiter(irl.max, irl.interval)
    }
    return irl.limiters[ip]
}
```

---

## Senior mindset: "Rate limit at the edge"

Rate limiting should happen **before** your service logic, not inside it. That's why it's middleware — it runs first.

- **Global rate limit** — protects your server from total overload
- **Per-IP rate limit** — protects against a single abusive client
- **Per-endpoint rate limit** — protects expensive operations (e.g., URL creation) while keeping cheap ones (redirects) fast

---

## Summary

| Concept | Code |
|---------|------|
| Token bucket | `Allow()` returns true/false |
| Rate limit middleware | Check `Allow()`, return 429 if false |
| Per-IP limit | Map of IP → limiter |
| Retry-After header | Tell client when to retry |

Open DAY26/DRILL.md.
