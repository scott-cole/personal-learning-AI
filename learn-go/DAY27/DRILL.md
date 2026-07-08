# DAY27: Drill — "Restructure the URL Shortener"

Move the URL shortener into the standard Go project layout.

## Structure to create

```
DAY27/
├── go.mod
├── cmd/
│   └── server/
│       └── main.go           → wire everything, start server
└── internal/
    ├── handler/
    │   └── handler.go        → HTTP handlers
    ├── service/
    │   └── service.go        → business logic
    ├── storage/
    │   ├── repository.go     → Repository interface + URL model
    │   └── memory.go         → MemoryRepository
    └── middleware/
        └── middleware.go     → logging, request ID, rate limiter
```

## Steps

1. Create the directory structure
2. Move each piece into its package
3. Update import paths

## internal/storage/repository.go

```go
package storage

import "time"

type URL struct {
    Code         string
    OriginalURL  string
    CreatedAt    time.Time
    AccessCount  int
}

type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
}
```

## internal/storage/memory.go

```go
package storage

import (
    "errors"
    "sync"
)

type MemoryRepository struct {
    mu   sync.RWMutex
    data map[string]URL
}

func NewMemoryRepository() *MemoryRepository {
    return &MemoryRepository{data: make(map[string]URL)}
}

func (r *MemoryRepository) Save(url URL) error {
    r.mu.Lock()
    defer r.mu.Unlock()
    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepository) FindByCode(code string) (URL, error) {
    r.mu.RLock()
    defer r.mu.RUnlock()
    url, ok := r.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}
```

## internal/service/service.go

```go
package service

import (
    "crypto/rand"
    "errors"
    "math/big"
    "strings"
    "learn-go/DAY27/internal/storage"
)

const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

type Service struct {
    repo storage.Repository
}

func NewService(repo storage.Repository) *Service {
    return &Service{repo: repo}
}

func (s *Service) Shorten(originalURL string) (storage.URL, error) {
    if !strings.HasPrefix(originalURL, "http://") && !strings.HasPrefix(originalURL, "https://") {
        return storage.URL{}, errors.New("url must start with http:// or https://")
    }
    code, err := generateCode(8)
    if err != nil {
        return storage.URL{}, err
    }
    url := storage.URL{Code: code, OriginalURL: originalURL}
    err = s.repo.Save(url)
    if err != nil {
        return storage.URL{}, err
    }
    return url, nil
}

func (s *Service) Resolve(code string) (storage.URL, error) {
    return s.repo.FindByCode(code)
}

func generateCode(length int) (string, error) {
    result := make([]byte, length)
    for i := range result {
        n, err := rand.Int(rand.Reader, big.NewInt(int64(len(charset))))
        if err != nil {
            return "", err
        }
        result[i] = charset[n.Int64()]
    }
    return string(result), nil
}
```

## internal/handler/handler.go

```go
package handler

import (
    "encoding/json"
    "net/http"
    "learn-go/DAY27/internal/service"
)

type Handler struct {
    svc *service.Service
}

func New(svc *service.Service) *Handler {
    return &Handler{svc: svc}
}

func (h *Handler) Shorten(w http.ResponseWriter, r *http.Request) {
    var req struct {
        URL string `json:"url"`
    }
    if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
        http.Error(w, "invalid JSON", http.StatusBadRequest)
        return
    }
    result, err := h.svc.Shorten(req.URL)
    if err != nil {
        http.Error(w, err.Error(), http.StatusBadRequest)
        return
    }
    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)
    json.NewEncoder(w).Encode(result)
}

func (h *Handler) Resolve(w http.ResponseWriter, r *http.Request) {
    code := r.PathValue("code")
    url, err := h.svc.Resolve(code)
    if err != nil {
        http.Error(w, "not found", http.StatusNotFound)
        return
    }
    http.Redirect(w, r, url.OriginalURL, http.StatusFound)
}
```

## cmd/server/main.go

```go
package main

import (
    "log"
    "net/http"
    "learn-go/DAY27/internal/handler"
    "learn-go/DAY27/internal/service"
    "learn-go/DAY27/internal/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)
    h := handler.New(svc)

    mux := http.NewServeMux()
    mux.HandleFunc("POST /shorten", h.Shorten)
    mux.HandleFunc("GET /{code}", h.Resolve)

    log.Println("Starting server on :8080")
    log.Fatal(http.ListenAndServe(":8080", mux))
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY27
go mod init learn-go/DAY27
go run ./cmd/server/
```

Test with curl as usual.
