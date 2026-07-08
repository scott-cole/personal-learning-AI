# DAY26: Drill — "Build a round-robin load balancer"

Build a load balancer that distributes requests across multiple Go servers.

## Part 1 — Create backend servers

Create `backend.go`:

```go
package main

import (
    "fmt"
    "os"
    "net/http"
)

func main() {
    port := os.Args[1]
    name := os.Args[2]

    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Response from %s (port %s)\n", name, port)
    })

    fmt.Printf("Backend %s on :%s\n", name, port)
    http.ListenAndServe(":"+port, nil)
}
```

## Part 2 — Create the load balancer

Create `lb.go`:

```go
package main

import (
    "fmt"
    "net/http"
    "net/http/httputil"
    "net/url"
    "sync/atomic"
)

type LoadBalancer struct {
    backends []*url.URL
    counter  uint64
}

func (lb *LoadBalancer) Next() *url.URL {
    n := atomic.AddUint64(&lb.counter, 1)
    return lb.backends[n%uint64(len(lb.backends))]
}

func (lb *LoadBalancer) ServeHTTP(w http.ResponseWriter, r *http.Request) {
    target := lb.Next()
    fmt.Printf("Forwarding to %s\n", target.Host)
    httputil.NewSingleHostReverseProxy(target).ServeHTTP(w, r)
}

func main() {
    lb := &LoadBalancer{
        backends: []*url.URL{
            {Host: "localhost:3001", Scheme: "http"},
            {Host: "localhost:3002", Scheme: "http"},
            {Host: "localhost:3003", Scheme: "http"},
        },
    }

    fmt.Println("Load balancer on :8080")
    http.ListenAndServe(":8080", lb)
}
```

## Part 3 — Run everything

```bash
# Terminal 1-3: Start backends
go run backend.go 3001 Alpha
go run backend.go 3002 Beta
go run backend.go 3003 Gamma

# Terminal 4: Start load balancer
go run lb.go

# Terminal 5: Test round-robin
for i in {1..6}; do
  curl -s http://localhost:8080
  echo ""
done
# Output should alternate: Alpha, Beta, Gamma, Alpha, Beta, Gamma
```

## Part 4 — Simulate a server failure

```bash
# Kill one backend (e.g., port 3001)
# The load balancer still sends requests to it!
# We need health checks...

# For now, just observe what happens
curl -s http://localhost:8080
# Some requests will fail (connection refused)
```

## Part 5 — Add basic health checks

Modify the load balancer to skip backends that fail:

```go
// Before forwarding, check if the backend is healthy
// Simplified: try a TCP connection first
func (lb *LoadBalancer) getHealthyBackend() *url.URL {
    for i := 0; i < len(lb.backends); i++ {
        target := lb.Next()
        conn, err := net.DialTimeout("tcp", target.Host, 500*time.Millisecond)
        if err == nil {
            conn.Close()
            return target
        }
        fmt.Printf("Backend %s is down, trying next...\n", target.Host)
    }
    return nil
}
```

## Deliverables

1. The round-robin output (Alpha, Beta, Gamma cycling)
2. What happened when you killed a backend?
3. Your health check implementation

## Hints

<details>
<summary>Click for hints</summary>

- `sync/atomic.AddUint64` is thread-safe — multiple requests won't collide
- `httputil.NewSingleHostReverseProxy` forwards everything automatically
- A "connection refused" error means the backend is down
- Real load balancers use passive health checks (tracking 5xx responses) and active health checks (periodic pings)
- nginx handles all this with a simple config — this exercise shows what happens under the hood
</details>
