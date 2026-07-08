# DAY18: Drill — "Observe caching in the wild"

Watch caching headers flow between your browser and real servers.

## Part 1 — Cache-Control on a static asset

```bash
# Fetch a logo that's cached for a long time
curl -I https://www.google.com/images/branding/googlelogo/2x/googlelogo_color_272x92dp.png

# What does Cache-Control say?
# Look for: max-age or expires
```

## Part 2 — No-cache on dynamic content

```bash
# Fetch an API response
curl -I https://api.github.com/users/scott

# Compare Cache-Control with the Google logo
# What's different?
```

## Part 3 — ETags with GitHub API

```bash
# Get the ETag
RESPONSE=$(curl -I -s https://api.github.com/users/scott)
ETAG=$(echo "$RESPONSE" | grep -i etag | sed 's/.*: //')
echo "ETag: $ETAG"

# Use it for a conditional request
curl -v -H "If-None-Match: $ETAG" https://api.github.com/users/scott 2>&1 | grep -E "< HTTP|< content-length|etag"
# Expected: 304 Not Modified (zero body)
```

## Part 4 — Browser DevTools cache inspection

1. Open DevTools → Network tab
2. Load any website
3. Click on a CSS or JS file
4. Look at the **Headers** tab for `Cache-Control`
5. Check the **Size** column — cached resources show `(disk cache)` or `(memory cache)`
6. Reload the page — which resources were served from cache?

## Part 5 — Build a caching server in Go

```go
package main

import (
    "crypto/sha256"
    "fmt"
    "net/http"
    "time"
)

var visits = 0

func handler(w http.ResponseWriter, r *http.Request) {
    // ETag based on current visit count
    etag := fmt.Sprintf(`"%x"`, sha256.Sum256([]byte(fmt.Sprintf("%d", visits))))

    if r.Header.Get("If-None-Match") == etag {
        w.WriteHeader(http.StatusNotModified)
        return
    }

    visits++
    w.Header().Set("Cache-Control", "public, max-age=5")
    w.Header().Set("ETag", etag)
    fmt.Fprintf(w, "Visit count: %d", visits)
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("Server on :8080")
    http.ListenAndServe(":8080", nil)
}
```

Run it, then:

```bash
# First request (both should be same — cache hasn't changed)
curl -v http://localhost:8080 2>&1 | grep -E "< HTTP|< etag|< cache-control|Visit"
sleep 6  # Wait for cache to expire
curl -v -H "If-None-Match: \"the-etag-from-before\"" http://localhost:8080 2>&1
```

## Deliverables

1. Cache-Control values from Parts 1 and 2
2. Did you get a 304 in Part 3?
3. What resources does your favourite site cache?
4. Your Go server output

## Hints

<details>
<summary>Click for hints</summary>

- Static assets (images, CSS, JS) have long `max-age` (months to years)
- API responses often have `no-cache` or short `max-age`
- `304 Not Modified` has no body — `Content-Length: 0`
- In DevTools, the "Size" column shows `(disk cache)` or `(memory cache)` for cached resources
</details>
