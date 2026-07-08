# DAY14: Week 2 Project — "Full URL Shortener v1"

This is your Week 2 project. Bring together everything you've built:

- **Repository layer** — data access
- **Service layer** — business logic
- **Handlers** — HTTP endpoints
- **JSON** — request/response format
- **Middleware** — logging + request ID

## The spec

```
POST /shorten   {"url": "https://example.com"}
  → 201 {"code": "abc123", "original_url": "...", "short_url": "http://localhost:8080/abc123"}

GET /{code}
  → 302 Redirect to the original URL

GET /{code}/stats
  → 200 {"code": "abc123", "original_url": "...", "access_count": 5, "created_at": "..."}
```

## New feature: click tracking

Add an `AccessCount` field to the `URL` struct and increment it every time someone resolves a code:

```go
type URL struct {
    Code         string
    OriginalURL  string
    CreatedAt    time.Time
    AccessCount  int
}
```

## Structure

```
DAY14/
├── go.mod
├── main.go              → server setup + handlers
├── storage/
│   └── memory.go        → Repository + MemoryRepository
└── service/
    └── service.go       → Service with Shorten, Resolve, Stats
```

## What to implement

### storage/memory.go
Add `AccessCount` to URL struct. Add a `List()` method and `IncrementAccess(code string)`.

### service/service.go  
Add `Stats(code string)` method that returns the URL with access count.
In `Resolve`, call `repo.IncrementAccess(code)` before returning.

### main.go
Add `GET /{code}/stats` handler that returns JSON with code, original URL, access count, created_at.

## Full file listing

### storage/memory.go

```go
package storage

import (
    "errors"
    "time"
)

type URL struct {
    Code         string
    OriginalURL  string
    CreatedAt    time.Time
    AccessCount  int
}

type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
    IncrementAccess(code string) error
    List() ([]URL, error)
}

type MemoryRepository struct {
    data map[string]URL
}

func NewMemoryRepository() *MemoryRepository {
    return &MemoryRepository{data: make(map[string]URL)}
}

func (r *MemoryRepository) Save(url URL) error {
    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepository) FindByCode(code string) (URL, error) {
    url, ok := r.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}

func (r *MemoryRepository) IncrementAccess(code string) error {
    url, ok := r.data[code]
    if !ok {
        return errors.New("not found")
    }
    url.AccessCount++
    r.data[code] = url
    return nil
}

func (r *MemoryRepository) List() ([]URL, error) {
    var urls []URL
    for _, url := range r.data {
        urls = append(urls, url)
    }
    return urls, nil
}
```

## Test it

```bash
cd ~/Dev/learn-go/DAY14
go run main.go
```

In another terminal:

```bash
# Shorten
curl -s -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'

# Resolve (replace CODE)
curl -v http://localhost:8080/CODE

# Check stats (replace CODE)
curl -s http://localhost:8080/CODE/stats | jq .
```

The stats endpoint should return JSON with `access_count` showing how many times that code was resolved.
