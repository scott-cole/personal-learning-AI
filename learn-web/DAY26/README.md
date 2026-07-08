# DAY26: Load Balancers — Multiple Checkout Lanes

---

## The anecdote: "Load balancing is multiple checkout lanes"

A supermarket with one checkout lane:

| Single lane | Multiple lanes |
|-------------|----------------|
| Everyone queues in one line | Cashier #1, Cashier #2, Cashier #3 |
| If the cashier is slow, everyone waits | Customers go to the shortest queue |
| If the cashier leaves, the store closes | One cashier can take a break — store stays open |
| Queue wraps around the store (10 minutes wait) | 3 lanes, 1-2 minute wait |

A **load balancer** distributes incoming requests across multiple backend servers — just like multiple checkout lanes.

---

## Why load balance?

| Reason | Without load balancer | With load balancer |
|--------|----------------------|-------------------|
| Traffic spike | Server crashes | Distributed across 3+ servers |
| Server failure | Site goes down | Other servers handle traffic |
| Maintenance | Take site down | Drain one server, update it, bring it back |
| Performance | Single server maxed out | Horizontal scaling |

---

## Load balancing algorithms

### Round-robin

```
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
Request 4 → Server A
Request 5 → Server B
...
```

Simple, fair, but doesn't account for server load.

### Least connections

```
Server A: 10 active connections
Server B: 2 active connections  → Send next request here
Server C: 7 active connections
```

Better when requests have different processing times.

### IP hash (sticky sessions)

```
Client 192.168.1.1 → Server A (always)
Client 192.168.1.2 → Server B (always)
Client 192.168.1.3 → Server C (always)
```

Hash the client IP so the same client always goes to the same server. Useful for session-based apps.

### Weighted round-robin

```
Server A: weight 5 (handles 5 requests)
Server B: weight 1 (handles 1 request)
Server C: weight 3 (handles 3 requests)
```

For servers with different capacities.

---

## Sticky sessions (session affinity)

Some apps store session data in memory on a specific server. The load balancer must send the same client to the same server — this is **stickiness**.

```nginx
upstream backend {
    ip_hash;  # Same IP → same backend
    server 10.0.0.1:3000;
    server 10.0.0.2:3000;
    server 10.0.0.3:3000;
}
```

**Better approach:** Use a shared session store (Redis) so any server can handle any request — no stickiness needed.

---

## Health checks

The load balancer periodically checks if backends are alive:

```nginx
upstream backend {
    server 10.0.0.1:3000;
    server 10.0.0.2:3000;
    server 10.0.0.3:3000;

    # Active health check (nginx Plus)
    # check interval=5000 rise=2 fall=3 timeout=2000;
}

# Passive health check (built-in)
# If a backend returns 5xx 3 times, mark it down
```

---

## Load balancing in Go

```go
package main

import (
    "fmt"
    "net/http"
    "net/http/httputil"
    "net/url"
    "sync/atomic"
)

type RoundRobin struct {
    backends []*url.URL
    counter  uint64
}

func (rb *RoundRobin) Next() *url.URL {
    n := atomic.AddUint64(&rb.counter, 1)
    return rb.backends[n%uint64(len(rb.backends))]
}

func main() {
    rb := &RoundRobin{
        backends: []*url.URL{
            {Host: "localhost:3000", Scheme: "http"},
            {Host: "localhost:3001", Scheme: "http"},
            {Host: "localhost:3002", Scheme: "http"},
        },
    }

    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        target := rb.Next()
        proxy := httputil.NewSingleHostReverseProxy(target)
        proxy.ServeHTTP(w, r)
    })

    fmt.Println("Load balancer on :8080")
    http.ListenAndServe(":8080", handler)
}
```

---

## Senior mindset: "Design for no stickiness"

A junior engineer uses sticky sessions because "it works." A senior engineer uses a shared session store so any server can handle any request.

**Stateless backends are better backends.** If your backend has no local state, you don't need sticky sessions, and you gain full flexibility in load balancing.

---

## Summary

| Algorithm | How it works | Best for |
|-----------|-------------|----------|
| Round-robin | Cycles through servers | Equal-capacity servers |
| Least connections | Picks least busy server | Variable request durations |
| IP hash | Same client → same server | Session-based apps |
| Weighted | Distributes by weight | Unequal capacity servers |

| Feature | Purpose |
|---------|---------|
| Health checks | Detect and remove failing servers |
| Sticky sessions | Route same client to same server |
| Draining | Stop sending new requests to a server about to be removed |
