# DAY23: Drill — "Kill it gently"

Build a server that handles shutdown gracefully and observe the difference.

## Part 1 — The bad server (no graceful shutdown)

Create `bad-server.go`:

```go
package main

import (
    "fmt"
    "net/http"
    "time"
)

func slowHandler(w http.ResponseWriter, r *http.Request) {
    time.Sleep(5 * time.Second)
    fmt.Fprintf(w, "Finished after 5 seconds\n")
}

func main() {
    http.HandleFunc("/slow", slowHandler)
    fmt.Println("Bad server on :8080 (no graceful shutdown)")
    http.ListenAndServe(":8080", nil)
}
```

Run it, send a slow request, and press Ctrl+C during the request:

```bash
go run bad-server.go &
curl http://localhost:8080/slow &
# Quickly press Ctrl+C — the curl gets a connection reset
```

## Part 2 — The good server (graceful shutdown)

Create `good-server.go`:

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

func slowHandler(w http.ResponseWriter, r *http.Request) {
    time.Sleep(5 * time.Second)
    fmt.Fprintf(w, "Finished after 5 seconds\n")
}

func main() {
    mux := http.NewServeMux()
    mux.HandleFunc("/slow", slowHandler)
    mux.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Fast response\n")
    })

    server := &http.Server{Addr: ":8081", Handler: mux}

    go func() {
        fmt.Println("Good server on :8081")
        if err := server.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalf("Error: %v", err)
        }
    }()

    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    fmt.Println("\nShutting down gracefully — waiting for in-flight requests...")
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    if err := server.Shutdown(ctx); err != nil {
        log.Fatalf("Forced shutdown: %v", err)
    }
    fmt.Println("Server stopped cleanly")
}
```

## Part 3 — Compare the behaviour

```bash
# Terminal 1: Bad server
go run bad-server.go &
curl http://localhost:8080/slow
# Press Ctrl+C during the request — what happens?

# Terminal 2: Good server
go run good-server.go &
curl http://localhost:8081/slow
# Press Ctrl+C during the request — what happens?
```

## Part 4 — Add shutdown hooks

Create `clean-server.go` that:

1. Prints "Saving state..." when shutdown starts
2. Waits 2 seconds (simulating cleanup)
3. Prints "State saved. Closing database..."
4. Prints "Database closed. Goodbye!"
5. Then exits

## Part 5 — Test with signals

```bash
# Use kill instead of Ctrl+C
go run good-server.go &
PID=$!
sleep 1

# Send a slow request
curl http://localhost:8081/slow &

# Kill gracefully
kill -TERM $PID
# Server should wait for the slow request

# Wait and check if server exits cleanly
wait $PID
echo "Server exited with code: $?"
```

## Deliverables

1. What happens when you Ctrl+C the bad server during a request?
2. What happens when you Ctrl+C the good server during a request?
3. Your clean-server.go with shutdown hooks
4. The exit code of the good server after SIGTERM

## Hints

<details>
<summary>Click for hints</summary>

- The bad server shows "connection reset by peer" on the client
- The good server lets the curl finish before exiting
- `server.Shutdown(ctx)` blocks until all connections are done or the context times out
- Use `go run server.go &` to run in background, then `kill %1` to send SIGTERM
- `http.ErrServerClosed` is the expected error after `Shutdown()` is called
</details>
