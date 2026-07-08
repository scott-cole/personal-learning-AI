# DAY25: Drill — "Set up a reverse proxy"

Run a Go app behind a reverse proxy (using nginx or a Go-based proxy).

## Part 1 — Simple Go app without a proxy

Create `app.go`:

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello from app!\n")
    fmt.Fprintf(w, "RemoteAddr: %s\n", r.RemoteAddr)
    fmt.Fprintf(w, "X-Forwarded-For: %s\n", r.Header.Get("X-Forwarded-For"))
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("App on :3000")
    http.ListenAndServe(":3000", nil)
}
```

## Part 2 — Write a Go reverse proxy

Create `proxy.go`:

```go
package main

import (
    "fmt"
    "net/http"
    "net/http/httputil"
    "net/url"
)

func main() {
    target, _ := url.Parse("http://localhost:3000")
    proxy := httputil.NewSingleHostReverseProxy(target)

    // Custom handler to modify headers
    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        r.Header.Set("X-Forwarded-For", r.RemoteAddr)
        r.Header.Set("X-Forwarded-Proto", "http")
        proxy.ServeHTTP(w, r)
    })

    fmt.Println("Reverse proxy on :8080 → :3000")
    http.ListenAndServe(":8080", handler)
}
```

## Part 3 — Run and test

```bash
# Terminal 1: Start the app
go run app.go

# Terminal 2: Start the proxy
go run proxy.go

# Terminal 3: Test direct vs proxy
curl http://localhost:3000
# RemoteAddr will be 127.0.0.1:xxxx

curl http://localhost:8080
# RemoteAddr will be 127.0.0.1:yyyy (proxy)
# X-Forwarded-For will be 127.0.0.1:yyyy (client)
```

## Part 4 — Add routing to the proxy

Modify the proxy to route `/api/` to one port and `/` to another:

```go
package main

import (
    "fmt"
    "net/http"
    "net/http/httputil"
    "net/url"
    "strings"
)

func main() {
    apiTarget, _ := url.Parse("http://localhost:3001")
    apiProxy := httputil.NewSingleHostReverseProxy(apiTarget)

    webTarget, _ := url.Parse("http://localhost:3000")
    webProxy := httputil.NewSingleHostReverseProxy(webTarget)

    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        if strings.HasPrefix(r.URL.Path, "/api/") {
            apiProxy.ServeHTTP(w, r)
        } else {
            webProxy.ServeHTTP(w, r)
        }
    })

    fmt.Println("Smart proxy on :8080")
    http.ListenAndServe(":8080", handler)
}
```

You'd need a second app on port 3001, but this shows the concept.

## Part 5 — Simulate an nginx-like setup with curl

```bash
# See what headers the proxy adds
curl -v http://localhost:8080/ 2>&1 | grep -E "Forwarded|RemoteAddr"
```

## Deliverables

1. Output from direct app access (port 3000)
2. Output from proxy access (port 8080)
3. What's different about the RemoteAddr and X-Forwarded-For?

## Hints

<details>
<summary>Click for hints</summary>

- `httputil.NewSingleHostReverseProxy` is Go's built-in reverse proxy — no dependencies needed
- The proxy modifies the request before forwarding it to the backend
- Without the proxy, `r.RemoteAddr` is the client's IP
- With the proxy, `r.RemoteAddr` is the proxy's IP, but `X-Forwarded-For` has the real client IP
- If you have nginx installed, try writing a real nginx config instead
</details>
