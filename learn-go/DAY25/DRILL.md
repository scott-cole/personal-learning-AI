# DAY25: Drill — "Add Graceful Shutdown"

Add graceful shutdown to the URL shortener.

## Structure

```
DAY25/
├── main.go              → graceful shutdown
├── storage/
│   └── memory.go        → from DAY24
└── service/
    └── service.go       → from DAY10
```

## main.go

```go
package main

import (
    "context"
    "encoding/json"
    "fmt"
    "log"
    "net/http"
    "os"
    "os/signal"
    "syscall"
    "time"
    "learn-go/DAY25/service"
    "learn-go/DAY25/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    mux := http.NewServeMux()

    mux.HandleFunc("POST /shorten", func(w http.ResponseWriter, r *http.Request) {
        var req struct {
            URL string `json:"url"`
        }
        if err := json.NewDecoder(r.Body).Decode(&req); err != nil {
            http.Error(w, "bad request", http.StatusBadRequest)
            return
        }
        // Simulate slow request
        time.Sleep(2 * time.Second)
        result, err := svc.Shorten(req.URL)
        if err != nil {
            http.Error(w, err.Error(), http.StatusBadRequest)
            return
        }
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusCreated)
        json.NewEncoder(w).Encode(result)
    })

    mux.HandleFunc("GET /{code}", func(w http.ResponseWriter, r *http.Request) {
        code := r.PathValue("code")
        url, err := svc.Resolve(code)
        if err != nil {
            http.Error(w, "not found", http.StatusNotFound)
            return
        }
        http.Redirect(w, r, url.OriginalURL, http.StatusFound)
    })

    srv := &http.Server{
        Addr:    ":8080",
        Handler: mux,
    }

    // Start server
    go func() {
        log.Printf("Starting server on %s", srv.Addr)
        if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalf("server error: %v", err)
        }
    }()

    // Wait for interrupt
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    log.Println("Shutting down gracefully...")

    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    if err := srv.Shutdown(ctx); err != nil {
        log.Fatalf("forced shutdown: %v", err)
    }

    log.Println("Server stopped")
}
```

## Test it

```bash
cd ~/Dev/learn-go/DAY25
go mod init learn-go/DAY25
go run main.go
```

In another terminal:

```bash
# Send a slow request
curl -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}' &

# Immediately press CTRL+C in the server terminal
```

Observe: the server waits for the in-flight request to complete before shutting down.

## Run it

```bash
cd ~/Dev/learn-go/DAY25
go run main.go
```
