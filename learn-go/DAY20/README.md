# DAY20: Error Wrapping & Logging — "A Stack of Post-It Notes"

---

## The anecdote: "Post-it notes on a problem"

A bug report comes in: "Save failed."

Where? Why? Which URL? Was it a duplicate? Network issue? Constraint violation?

Without context, you're guessing. With **error wrapping**, each layer adds a post-it note:

```
repository: save url "abc123": UNIQUE constraint failed
              ↑ wraps
service: shorten "https://example.com": repository: save url "abc123": UNIQUE constraint failed
              ↑ wraps
handler: POST /shorten: shorten "https://example.com": repository: save url "abc123": UNIQUE constraint failed
```

You can read the stack top-to-bottom: what happened → where → what caused it.

---

## Error wrapping in Go (Go 1.13+)

```go
import "fmt"

// Wrap an error with context
if err != nil {
    return fmt.Errorf("save url %q: %w", url.Code, err)
}
```

`%w` (lowercase) wraps the original error. The caller can still use `errors.Is` and `errors.As` to check the root cause.

`%v` (not `%w`) creates a new error with the message but loses the original:

```go
return fmt.Errorf("save url %q: %v", url.Code, err) // original error lost
```

---

## errors.Is and errors.As

```go
// errors.Is — check if ANY error in the chain matches a specific sentinel
if errors.Is(err, sql.ErrNoRows) {
    // handle "not found" case
}

// errors.As — check if ANY error in the chain is of a specific type
var sqlErr *mysql.MySQLError
if errors.As(err, &sqlErr) {
    // handle MySQL-specific error
}
```

---

## Logging best practices

```go
// In your handler/service
log.Printf("error shortening URL %q: %v", originalURL, err)
```

Structured logging (key=value pairs) is even better, but start with `log.Printf`.

**When to log:**
- At the **handler** level — log every request + error
- At the **service** level — log business logic decisions
- **Never** log at the repository level — let the caller decide

**What to log:**
- Request ID (from middleware) — to correlate logs
- What happened — "shortened URL", "resolved code"
- Error context — not just "error", but "what were we doing"

---

## A logging middleware (from DAY13)

```go
func loggingMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        start := time.Now()
        next.ServeHTTP(w, r)
        log.Printf("%s %s %s", r.Method, r.URL.Path, time.Since(start))
    })
}
```

---

## Senior mindset: "Wrap at every boundary"

Wrap errors at **layer boundaries**, not inside the same function:

```go
// repository — wrap with operation + identifier
return fmt.Errorf("save %q: %w", url.Code, err)

// service — wrap with business operation
return fmt.Errorf("shorten: %w", err)

// handler — wrap with HTTP context
log.Printf("POST /shorten: %v", err)
```

Every boundary gets a post-it note. The root cause is preserved with `%w`.

---

## Summary

| Ops | Code | Preserves root cause? |
|--------|------|----------------------|
| Add context | `fmt.Errorf("msg: %w", err)` | Yes |
| Add context (lose cause) | `fmt.Errorf("msg: %v", err)` | No |
| Check root cause | `errors.Is(err, sentinel)` | — |
| Extract typed error | `errors.As(err, &target)` | — |

Open DAY20/DRILL.md.
