# DAY25: Graceful Shutdown — "Restarting Without Dropping Requests"

---

## The anecdote: "Closing a restaurant"

You don't flip the "OPEN" sign to "CLOSED" and walk out while customers are eating.

You:
1. Flip the sign (stop accepting new customers)
2. Let current customers finish (in-flight requests)
3. Clean up (close database connections)
4. Lock the door (exit)

This is **graceful shutdown**.

---

## Without graceful shutdown

```go
func main() {
    http.ListenAndServe(":8080", mux)
    // CTRL+C → process killed immediately
    // Current requests are dropped
    // Database connections are cut
    // File descriptors leaked
}
```

## With graceful shutdown

```go
func main() {
    srv := &http.Server{Addr: ":8080", Handler: mux}

    // Start server in a goroutine
    go func() {
        if err := srv.ListenAndServe(); err != nil && err != http.ErrServerClosed {
            log.Fatalf("server error: %v", err)
        }
    }()

    // Wait for interrupt signal
    quit := make(chan os.Signal, 1)
    signal.Notify(quit, syscall.SIGINT, syscall.SIGTERM)
    <-quit

    log.Println("Shutting down gracefully...")

    // Give active requests 30 seconds to complete
    ctx, cancel := context.WithTimeout(context.Background(), 30*time.Second)
    defer cancel()

    if err := srv.Shutdown(ctx); err != nil {
        log.Fatalf("forced shutdown: %v", err)
    }

    log.Println("Server stopped")
}
```

---

## How it works

1. **`signal.Notify`** — tells Go "send me SIGINT (CTRL+C) and SIGTERM (kill) on this channel"
2. **`<-quit`** — blocks until a signal arrives
3. **`srv.Shutdown(ctx)`** — stops accepting new requests, waits for in-flight ones to finish
4. **Context timeout** — if requests don't finish in 30 seconds, force shutdown

---

## Closing the database

```go
db, _ := sql.Open("sqlite3", cfg.DBPath)
defer db.Close()

// ... in the shutdown code:
if err := srv.Shutdown(ctx); err != nil {
    log.Fatalf("forced shutdown: %v", err)
}
if err := db.Close(); err != nil {
    log.Printf("db close error: %v", err)
}
```

---

## Senior mindset: "Always shutdown gracefully in production"

A non-graceful shutdown in production means:
- Dropped HTTP requests (bad UX)
- Corrupted database state (bad data)
- Leaked file descriptors (bad ops)

Every HTTP server you deploy must handle shutdown.

---

## Summary

| Component | Purpose |
|-----------|---------|
| `signal.Notify` | Capture OS signals (CTRL+C, kill) |
| `srv.Shutdown(ctx)` | Stop accepting, wait for in-flight |
| `context.WithTimeout` | Don't wait forever |
| `srv.ListenAndServe` in goroutine | Non-blocking start |

Open DAY25/DRILL.md.
