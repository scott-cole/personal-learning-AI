# DAY30: Final Project — Build the URL Shortener in Go

---

## The anecdote: "This is your graduation"

Day 1 you learned what HTTP is. Now, 30 days later, you're building a complete web service from scratch.

Everything connects:
- HTTP methods (POST to create, GET to redirect, DELETE to remove)
- Status codes (201, 302, 400, 404, 429)
- Headers (Location for redirect, Content-Type for JSON)
- Rate limiting (protect your API)
- Graceful shutdown (handle SIGINT)
- Proper error responses

---

## The project

Build a URL shortener server with:

1. **POST /api/shorten** — create a short URL
2. **GET /api/{code}** — redirect to the original URL
3. **GET /api/urls** — list all URLs
4. **GET /api/urls/{id}** — get stats for a URL
5. **DELETE /api/urls/{id}** — delete a URL
6. In-memory storage
7. Rate limiting by IP
8. Graceful shutdown

---

## Starter code

```go
package main

import (
    "crypto/rand"
    "encoding/hex"
    "encoding/json"
    "fmt"
    "math/big"
    "net/http"
    "strings"
    "sync"
    "time"
)

// Data model
type URL struct {
    ID        string    `json:"id"`
    ShortCode string    `json:"short_code"`
    LongURL   string    `json:"long_url"`
    Clicks    int       `json:"clicks"`
    CreatedAt time.Time `json:"created_at"`
}

type Store struct {
    mu    sync.RWMutex
    urls  map[string]*URL   // short_code → URL
    byID  map[string]string // id → short_code
}

func NewStore() *Store {
    return &Store{
        urls:  make(map[string]*URL),
        byID:  make(map[string]string),
    }
}

func (s *Store) Create(longURL string) *URL {
    s.mu.Lock()
    defer s.mu.Unlock()

    id := generateID()
    code := generateShortCode()

    url := &URL{
        ID:        id,
        ShortCode: code,
        LongURL:   longURL,
        CreatedAt: time.Now(),
    }
    s.urls[code] = url
    s.byID[id] = code
    return url
}

func (s *Store) GetByCode(code string) *URL {
    s.mu.RLock()
    defer s.mu.RUnlock()
    return s.urls[code]
}

func (s *Store) GetAll() []*URL {
    s.mu.RLock()
    defer s.mu.RUnlock()
    result := make([]*URL, 0, len(s.urls))
    for _, url := range s.urls {
        result = append(result, url)
    }
    return result
}

func (s *Store) Delete(id string) bool {
    s.mu.Lock()
    defer s.mu.Unlock()
    code, ok := s.byID[id]
    if !ok {
        return false
    }
    delete(s.urls, code)
    delete(s.byID, id)
    return true
}

func (s *Store) IncrementClicks(code string) {
    s.mu.Lock()
    defer s.mu.Unlock()
    if u, ok := s.urls[code]; ok {
        u.Clicks++
    }
}

func generateID() string {
    b := make([]byte, 8)
    rand.Read(b)
    return "url_" + hex.EncodeToString(b)
}

func generateShortCode() string {
    const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"
    code := make([]byte, 7)
    for i := range code {
        n, _ := rand.Int(rand.Reader, big.NewInt(int64(len(charset))))
        code[i] = charset[n.Int64()]
    }
    return string(code)
}
```

---

## Handlers

```go
func NewHandler(store *Store, rl *RateLimiter) http.Handler {
    mux := http.NewServeMux()

    mux.HandleFunc("/api/shorten", func(w http.ResponseWriter, r *http.Request) {
        if r.Method != "POST" {
            writeError(w, http.StatusMethodNotAllowed, "METHOD_NOT_ALLOWED", "Use POST")
            return
        }
        if !rl.Allow(r.RemoteAddr) {
            w.Header().Set("Retry-After", "10")
            writeError(w, http.StatusTooManyRequests, "RATE_LIMITED", "Too many requests")
            return
        }

        var req struct {
            URL string `json:"url"`
        }
        if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
            writeError(w, http.StatusBadRequest, "INVALID_JSON", "Invalid request body")
            return
        }
        if req.URL == "" {
            writeError(w, http.StatusBadRequest, "MISSING_URL", "url field is required")
            return
        }
        if !strings.HasPrefix(req.URL, "http://") && !strings.HasPrefix(req.URL, "https://") {
            writeError(w, http.StatusUnprocessableEntity, "INVALID_URL", "URL must start with http:// or https://")
            return
        }

        url := store.Create(req.URL)
        writeJSON(w, http.StatusCreated, url)
    })

    mux.HandleFunc("/api/", func(w http.ResponseWriter, r *http.Request) {
        // Parse path: /api/{code}
        parts := strings.SplitN(strings.TrimPrefix(r.URL.Path, "/api/"), "/", 2)
        if len(parts) != 1 || parts[0] == "" {
            writeError(w, http.StatusNotFound, "NOT_FOUND", "Not found")
            return
        }
        code := parts[0]

        if r.Method == "GET" {
            url := store.GetByCode(code)
            if url == nil {
                writeError(w, http.StatusNotFound, "NOT_FOUND", "Short URL not found")
                return
            }
            store.IncrementClicks(code)
            w.Header().Set("Location", url.LongURL)
            w.Header().Set("Cache-Control", "no-cache")
            w.WriteHeader(http.StatusFound)
            return
        }

        writeError(w, http.StatusMethodNotAllowed, "METHOD_NOT_ALLOWED", "Method not allowed")
    })

    return mux
}
```

---

## What to implement

The code above is partial. You need to:

1. Complete all the handler functions (shorten, redirect, list, stats, delete)
2. Implement the rate limiter (from DAY22)
3. Add graceful shutdown (from DAY23)
4. Add JSON helper functions (`writeJSON`, `writeError`)
5. Add CORS headers
6. Add security headers
7. Test everything with curl

---

## Testing checklist

```bash
# Create a short URL
curl -X POST http://localhost:8080/api/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/very/long/url"}'

# Follow the redirect (302)
curl -v http://localhost:8080/api/abc123

# List all URLs
curl http://localhost:8080/api/urls

# Get stats for a URL
curl http://localhost:8080/api/urls/url_xxx

# Delete a URL
curl -X DELETE http://localhost:8080/api/urls/url_xxx

# Test rate limiting
for i in {1..110}; do
  curl -s -o /dev/null -w "%{http_code}\n" \
    -X POST http://localhost:8080/api/shorten \
    -H "Content-Type: application/json" \
    -d '{"url":"https://example.com/test"}'
done
# Expect 200 for first 100, then 429
```

---

## Senior mindset: "This is your portfolio piece"

A URL shortener is the "hello world" of web services — simple enough to build, complex enough to demonstrate skill.

**What makes it a senior project:**
- Proper error handling (not just returning 200 for everything)
- Rate limiting (you care about production readiness)
- Graceful shutdown (you think about deployment)
- Clean code structure (organised, testable)
- Good status codes and headers (you understand HTTP)

---

## Summary

| Component | Source |
|-----------|--------|
| HTTP handlers | This lesson |
| Rate limiter | DAY22 |
| Graceful shutdown | DAY23 |
| CORS middleware | DAY14 |
| Security headers | DAY28 |
| Data model | Your design from DAY29 |
