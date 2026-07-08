# DAY23: Graceful Shutdown — Closing Shop the Right Way

---

## The anecdote: "Graceful shutdown is closing a shop at the end of the day"

A well-run shop:

| Good shop (graceful) | Bad shop (forceful) |
|----------------------|---------------------|
| Flips sign to "CLOSED" at 5 PM | Locks the door at 5 PM sharp |
| Customers inside finish their purchase | Kicks everyone out immediately |
| Staff clean up, count the register | Abandons the register with money inside |
| Turns off lights last | Flips the breaker |
| Locks up and goes home | Doesn't lock up |

A graceful web server does the same:
1. Stop accepting new connections
2. Finish processing in-flight requests
3. Close database connections
4. Save any in-memory state
5. Exit cleanly

---

## The problem

```go
http.ListenAndServe(":8080", nil)
// When you Ctrl+C, the server just dies.
// In-flight requests are cut off.
// Database connections are dropped.
// Users get "Connection reset by peer".
// Data could be corrupted.
```

---

## OS signals

When you press Ctrl+C, the OS sends your process a **SIGINT** (Interrupt) signal. A `kill` command sends **SIGTERM** (Terminate).

```go
// Listen for signals
sigChan := make(chan os.Signal, 1)
signal.Notify(sigChan, syscall.SIGINT, syscall.SIGTERM)

// This blocks until a signal is received
sig := <-sigChan
fmt.Printf("Received %v, shutting down...\n", sig)
```

---

## Graceful shutdown in Go

```go
package main

import (
    "context"
    "fmt"
    "log"
    "net/http"
    "os"
    "os/signal"
    "syscall"
    "time"
)

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        // Simulate a slow request (3 seconds)
        time.Sleep(3 * time.Second)
        fmt.Fprintf(w, "Done!")
    })

    server := &http.Server{
        Addr:    ":8080",
        Handler: mux,
    }

    // Start server in a goroutine
    go func() {
        fmt.Println("Server listening on :8080")
        if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalf("Server error: %v", err)
        }
    }()

    // Wait for shutdown signal
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    fmt.Println("\nShutting down gracefully...")

    // Give in-flight requests 30 seconds to complete
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    if err := server.Shutdown(ctx); err != nil {
        log.Fatalf("Server forced to shutdown: %v", err)
    }

    fmt.Println("Server stopped cleanly")
}
```

Run it:

```bash
go run main.go &
# In another terminal:
curl http://localhost:8080  # This takes 3 seconds
# While it's running, press Ctrl+C in the server terminal
# The server waits for curl to finish before exiting
```

---

## What `Shutdown` does

```go
server.Shutdown(ctx)
```

1. Stops accepting new connections (listening socket closed)
2. Waits for active connections to finish (or context timeout)
3. Calls any registered shutdown hooks
4. Returns nil on success or context error on timeout

---

## Shutdown hooks (cleanup tasks)

```go
func main() {
    db, _ := sql.Open("postgres", "...")
    cache := redis.NewClient(...)

    // ... server setup ...

    <-quit
    fmt.Println("Shutting down...")

    // Gracefully close resources
    db.Close()
    cache.Close()
    // Flush any pending writes
    // Save state to disk
}
```

---

## How nginx and reverse proxies handle it

When you gracefully shut down a Go server behind nginx:
1. Nginx detects the connection close
2. Nginx marks the backend as "down"
3. Nginx sends new requests to other backends
4. Existing connections are drained
5. The Go server exits

---

## Senior mindset: "Always plan for shutdown"

A common junior mistake: writing servers that work perfectly in development but crash when deployed because of unhandled signals.

**Checklist for production servers:**
- ✅ Handle SIGINT and SIGTERM
- ✅ Set a reasonable shutdown timeout (30-60 seconds)
- ✅ Close database connections
- ✅ Flush logs
- ✅ Release any locks
- ✅ Signal to load balancer that we're draining

---

## Summary

| Signal | Source | Meaning |
|--------|--------|---------|
| SIGINT | Ctrl+C | Interrupt from keyboard |
| SIGTERM | `kill` or orchestrator | Terminate gracefully |
| SIGKILL | `kill -9` | Force kill (can't be caught) |

| Method | Purpose |
|--------|---------|
| `signal.Notify(ch, sigs...)` | Register signal handler |
| `server.Shutdown(ctx)` | Graceful HTTP server shutdown |
| `http.ErrServerClosed` | Expected error after shutdown |
