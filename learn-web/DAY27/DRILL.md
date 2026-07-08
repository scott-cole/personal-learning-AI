# DAY27: Drill — "Trace CDN behaviour"

Use curl and dig to observe how CDNs work.

## Part 1 — Find a site behind a CDN

Many major sites use CDNs. Let's check:

```bash
# Trace the route to a site
dig github.com A +short
# If you see multiple IPs, it's probably behind a load balancer or CDN

# Check the headers
curl -I https://www.cloudflare.com 2>&1 | grep -i "server\|cf-\|x-cache"

# Cloudflare adds headers like:
# server: cloudflare
# cf-ray: (unique request ID)
# cf-cache-status: HIT or MISS
```

## Part 2 — Observe cache status

```bash
# First request — likely a cache miss
curl -I https://www.cloudflare.com 2>&1 | grep -i "cf-cache-status"

# Second request — likely a cache hit
curl -I https://www.cloudflare.com 2>&1 | grep -i "cf-cache-status"
```

## Part 3 — Geographic latency test

Simulate what a CDN solves:

```bash
# Time a request to a US-based server (from wherever you are)
time curl -s -o /dev/null https://www.google.com

# Time a request to a server close to you
time curl -s -o /dev/null https://localhost:8080

# The difference is mostly network latency (distance)
```

## Part 4 — Set Cache-Control headers for CDN

Create `main.go`:

```go
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    // Static asset — cache on CDN for 1 day
    http.HandleFunc("/static/style.css", func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Type", "text/css")
        w.Header().Set("Cache-Control", "public, max-age=3600, s-maxage=86400")
        fmt.Fprintf(w, "body { color: red; }\n")
    })

    // Dynamic content — don't cache on CDN
    http.HandleFunc("/api/time", func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Type", "application/json")
        w.Header().Set("Cache-Control", "private, no-cache")
        fmt.Fprintf(w, `{"time":"%s"}`, time.Now().Format(time.RFC3339))
    })

    // HTML page — cache on CDN for 5 minutes
    http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Cache-Control", "public, max-age=300, s-maxage=300")
        fmt.Fprintf(w, "<html><body><h1>Hello at %s</h1></body></html>",
            time.Now().Format(time.RFC3339))
    })

    fmt.Println("Server on :8080")
    http.ListenAndServe(":8080", nil)
}
```

## Part 5 — Simulate a CDN with caching proxy

Run the server, then observe what a CDN would cache:

```bash
# Static asset — should show max-age=3600, s-maxage=86400
curl -v http://localhost:8080/static/style.css 2>&1 | grep -i "cache-control"

# Dynamic API — should show no-cache
curl -v http://localhost:8080/api/time 2>&1 | grep -i "cache-control"

# The CDN would look at these headers to decide what to cache
```

## Part 6 — Check a real CDN purge (bonus)

If you have a Cloudflare account:

```bash
# Purge the cache for your domain
curl -X POST "https://api.cloudflare.com/client/v4/zones/$ZONE_ID/purge_cache" \
  -H "Authorization: Bearer $CF_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"purge_everything":true}'
```

## Deliverables

1. What CDN headers does `cloudflare.com` return?
2. What's the difference in cache headers between `/static/style.css` and `/api/time`?
3. If you were running a CDN, which of these responses would you cache?

## Hints

<details>
<summary>Click for hints</summary>

- `cf-cache-status: HIT` means the response came from Cloudflare's cache
- `s-maxage` specifically targets shared caches (CDNs, proxies)
- `private` means the CDN should NOT cache the response — only the browser can
- Most CDNs only cache GET requests (not POST, PUT, DELETE)
- You can test if a site uses Cloudflare because it sets the `cf-ray` header
</details>
