# DAY22: Rate Limiting — Why, How, and the Algorithm

---

## The anecdote: "Rate limiting is a turnstile at a stadium"

A football stadium has turnstiles at every entrance:

| Stadium | Rate limiting |
|---------|---------------|
| Only 1 person per second through each turnstile | 1 request per second per client |
| If you try to push through faster, the turnstile locks | Server returns `429 Too Many Requests` |
| Season ticket holders get a dedicated entrance | Higher rate limits for authenticated users |
| After the match, everyone leaves at once — the turnstile lets a burst through | Burst capacity |
| The turnstile resets every minute | Rate limit window |

Without turnstiles, fans would stampede. Without rate limiting, servers get overwhelmed.

---

## Why rate limit?

| Problem | Impact |
|---------|--------|
| Malicious bot | Scrapes all your data in minutes |
| Buggy client | Sends thousands of requests in a loop |
| DDoS attack | Overwhelms your server with traffic |
| Accidental traffic spike | Viral post causes unexpected load |
| Abuse | Free tier user hits your API 10,000 times/second |

---

## Rate limit headers (standard)

Most APIs include rate limit info in response headers:

```bash
curl -I https://api.github.com/users/scott
```

Look for:

```text
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 57
X-RateLimit-Reset: 1700000000
```

| Header | Meaning |
|--------|---------|
| `X-RateLimit-Limit` | Max requests per window |
| `X-RateLimit-Remaining` | How many you have left |
| `X-RateLimit-Reset` | Unix timestamp when the window resets |

---

## Token bucket algorithm (the most common)

Imagine a bucket that holds 10 tokens. Every request removes 1 token. Every second, a new token is added.

```
Bucket: [🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵] 10 tokens available
   ↓ Request consumes 1 token
Bucket: [🔵🔵🔵🔵🔵🔵🔵🔵🔵⬜] 9 tokens available
   ↓ Token refilled every second
Bucket: [🔵🔵🔵🔵🔵🔵🔵🔵🔵🔵] 10 tokens (if enough time passed)
```

**Burst capacity:** The bucket size = max burst. If you don't use any tokens for 10 seconds, you still only have 10 — you can't accumulate infinite burst.

**Steady-state rate:** The refill rate limits long-term average.

---

## Leaky bucket algorithm

Requests come in at any rate, but leave at a fixed rate.

```
Requests → [=========] → Fixed-rate output
           Leaky bucket
           (queue)
```

Used by rate limiters where **order matters** and you want to smooth out traffic spikes.

---

## Rate limiting in Go

```go
package main

import (
    "fmt"
    "net/http"
    "sync"
    "time"
)

type RateLimiter struct {
    mu        sync.Mutex
    visitors  map[string]*visitor
    rate      int           // max requests per window
    window    time.Duration // time window
}

type visitor struct {
    count    int
    lastSeen time.Time
}

func NewRateLimiter(rate int, window time.Duration) *RateLimiter {
    rl := &RateLimiter{
        visitors: make(map[string]*visitor),
        rate:     rate,
        window:   window,
    }
    // Clean up old entries every minute
    go func() {
        for {
            time.Sleep(time.Minute)
            rl.mu.Lock()
            for ip, v := range rl.visitors {
                if time.Since(v.lastSeen) > rl.window {
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

    v, exists := rl.visitors[ip]
    if !exists {
        rl.visitors[ip] = &visitor{count: 1, lastSeen: time.Now()}
        return true
    }

    // Reset if window has passed
    if time.Since(v.lastSeen) > rl.window {
        v.count = 1
        v.lastSeen = time.Now()
        return true
    }

    v.lastSeen = time.Now()
    if v.count >= rl.rate {
        return false
    }
    v.count++
    return true
}
```

---

## Senior mindset: "Rate limit at the edge, not the app"

Rate limiting should happen in your reverse proxy (nginx, Cloudflare, API gateway), not in your application code.

```nginx
# nginx rate limiting
limit_req_zone $binary_remote_addr zone=mylimit:10m rate=10r/s;

server {
    location /api/ {
        limit_req zone=mylimit burst=20 nodelay;
    }
}
```

**Why?** If a malicious request is blocked after reaching your app server, it still used CPU, memory, and database connections. Block at the edge to protect your infrastructure.

---

## Summary

| Concept | Implementation |
|---------|---------------|
| Why | Prevent abuse, protect servers, ensure fair use |
| Headers | `X-RateLimit-Limit`, `-Remaining`, `-Reset` |
| Token bucket | Bucket of N tokens, refilled at fixed rate |
| Leaky bucket | Input queue, fixed-rate output |
| Status code | `429 Too Many Requests` |
| Best practice | Rate limit at reverse proxy, not app code |
