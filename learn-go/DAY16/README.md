# DAY16: database/sql + SQLite — "A Phone Line to the Database"

---

## The anecdote: "database/sql is a phone line"

You have a phone. You can call anyone who also has a phone (landline, mobile, VoIP — doesn't matter).

`database/sql` is the phone. **Drivers** (like `go-sqlite3`) are the phone network. You just dial and talk.

```
Go code → database/sql (phone) → driver (network) → SQLite (person)
```

---

## Connecting to SQLite from Go

First, install the driver:

```bash
go mod init learn-go/DAY16
go get github.com/mattn/go-sqlite3
```

Then connect:

```go
package main

import (
    "database/sql"
    "log"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    db, err := sql.Open("sqlite3", "./url_shortener.db")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // Ping to verify the connection works
    err = db.Ping()
    if err != nil {
        log.Fatal(err)
    }
    log.Println("Connected!")
}
```

**Notice:** `_ "github.com/mattn/go-sqlite3"` — the blank import. The driver registers itself with `database/sql` as a side effect. You never call it directly.

---

## Three ways to run queries

### db.Exec — when you don't expect rows back

```go
result, err := db.Exec("INSERT INTO urls (code, original_url) VALUES (?, ?)", "abc123", "https://example.com")
```

`?` is a **placeholder**. Never build SQL with string concatenation (SQL injection risk).

### db.QueryRow — when you expect one row

```go
var originalURL string
err := db.QueryRow("SELECT original_url FROM urls WHERE code = ?", "abc123").Scan(&originalURL)
```

`.Scan()` reads the result into your Go variables. One call per row.

### db.Query — when you expect multiple rows

```go
rows, err := db.Query("SELECT code, original_url FROM urls")
if err != nil {
    log.Fatal(err)
}
defer rows.Close()

for rows.Next() {
    var code, originalURL string
    err := rows.Scan(&code, &originalURL)
    if err != nil {
        log.Fatal(err)
    }
    log.Printf("%s → %s", code, originalURL)
}
```

Always `defer rows.Close()` and always check `rows.Err()` after the loop.

---

## Why placeholders matter

```go
// ❌ NEVER do this — SQL injection
db.Exec("INSERT INTO urls (code) VALUES ('" + userInput + "')")

// ✅ Always use placeholders
db.Exec("INSERT INTO urls (code) VALUES (?)", userInput)
```

With placeholders, the database treats the value as data, not as part of the SQL command. Even if `userInput = "abc123'); DROP TABLE urls; --"`, it's just a string.

---

## Senior mindset: "Close what you open"

```go
db.Close()     // close the database when done
rows.Close()   // close the result set
```

Not closing connections = "too many open files" panic in production.

---

## Summary

| Operation | Method |
|-----------|--------|
| Run a query with no result | `db.Exec` |
| Get one row | `db.QueryRow(...).Scan(&vars)` |
| Get multiple rows | `db.Query` + `rows.Next()` + `rows.Scan` |
| Placeholder | `?` (not `$1` — that's PostgreSQL) |

Open DAY16/DRILL.md.
