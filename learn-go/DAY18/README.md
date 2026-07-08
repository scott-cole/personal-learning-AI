# DAY18: Repository with SQL — "Swapping the Filing Cabinet"

---

## The anecdote: "Same filing cabinet, different room"

You had a filing cabinet in your office (MemoryRepository). Now you're moving it to a warehouse (SQLite). The filing cabinet is the same — same drawers, same labels. Just a different room.

The `Repository` interface stays identical. You write a new implementation called `SQLRepository` that talks to SQLite instead of a map.

```
Handler → Service → Repository (interface)
                         ↑
              ┌──────────┴──────────┐
         MemoryRepository     SQLRepository
```

---

## The SQLRepository

```go
type SQLRepository struct {
    db *sql.DB
}

func NewSQLRepository(db *sql.DB) *SQLRepository {
    return &SQLRepository{db: db}
}

func (r *SQLRepository) Save(url URL) error {
    _, err := r.db.Exec(
        "INSERT INTO urls (code, original_url) VALUES (?, ?)",
        url.Code, url.OriginalURL,
    )
    return err
}

func (r *SQLRepository) FindByCode(code string) (URL, error) {
    var u URL
    err := r.db.QueryRow(
        "SELECT code, original_url, access_count, created_at FROM urls WHERE code = ?",
        code,
    ).Scan(&u.Code, &u.OriginalURL, &u.AccessCount, &u.CreatedAt)
    if err == sql.ErrNoRows {
        return URL{}, errors.New("not found")
    }
    return u, err
}
```

**Notice:** The `Save` method now uses `INSERT INTO` instead of `r.data[code] = url`. But from the service's perspective, nothing changed. Same interface, different implementation.

---

## sql.ErrNoRows

When `QueryRow` finds nothing, it returns `sql.ErrNoRows`. You check for it and return a domain-level error (`"not found"`):

```go
if err == sql.ErrNoRows {
    return URL{}, errors.New("not found")
}
```

The service layer never sees `sql.ErrNoRows`. It just sees a regular error.

---

## The service doesn't change

```go
func main() {
    // Memory version
    repo := memory.NewMemoryRepository()
    svc := service.NewService(repo)

    // SQL version — same service, different repo
    db, _ := sql.Open("sqlite3", "./url_shortener.db")
    repo := sqlrepo.NewSQLRepository(db)
    svc := service.NewService(repo)  // identical call
}
```

This is the entire point of the repository pattern and the L in SOLID.

---

## Senior mindset: "The interface is the contract"

The `Repository` interface is a promise:

```
"Give me a code, I'll give you a URL. I don't care how."
```

By coding to the interface, not the implementation, you can:
- Swap databases without touching business logic
- Test your service with an in-memory repo (fast)
- Test your SQL repo with a real DB (slow, but only in integration tests)

---

## Summary

| MemoryRepository | SQLRepository |
|-----------------|---------------|
| `r.data[code]` | `SELECT ... WHERE code = ?` |
| `r.data[code] = url` | `INSERT INTO urls ...` |
| Returns `errors.New("not found")` | Returns `sql.ErrNoRows` → mapped |
| No dependencies | Requires `*sql.DB` |

Open DAY18/DRILL.md.
