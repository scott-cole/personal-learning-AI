# DAY16: Drill — "Connect Go to SQLite"

Create a Go program that connects to SQLite, creates a table, inserts data, and queries it.

## Setup

```bash
cd ~/Dev/learn-go/DAY16
go mod init learn-go/DAY16
go get github.com/mattn/go-sqlite3
```

## main.go

```go
package main

import (
    "database/sql"
    "fmt"
    "log"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    db, err := sql.Open("sqlite3", "./url_shortener.db")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // TODO: Create the urls table
    // CREATE TABLE IF NOT EXISTS urls (
    //     id INTEGER PRIMARY KEY AUTOINCREMENT,
    //     code TEXT NOT NULL UNIQUE,
    //     original_url TEXT NOT NULL,
    //     access_count INTEGER NOT NULL DEFAULT 0,
    //     created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
    // )

    // TODO: Insert a URL

    // TODO: Query it back and print it

    // TODO: Query all URLs and list them
}
```

## Expected output

```
Inserted: abc123
Found: abc123 → https://example.com
All URLs:
abc123 → https://example.com
```

## Your task

Fill in the TODOs. Use `db.Exec` for create/insert, `db.QueryRow` for single row, `db.Query` + `rows.Next` for all rows.

Run it twice — the second time it should still work (use `CREATE TABLE IF NOT EXISTS`).

## Hints

- `db.Exec("CREATE TABLE IF NOT EXISTS ...")`
- `db.Exec("INSERT INTO urls (code, original_url) VALUES (?, ?)", code, url)`
- `db.QueryRow("SELECT code, original_url FROM urls WHERE code = ?", code).Scan(&c, &u)`
- `rows, err := db.Query("SELECT code, original_url FROM urls")`
- Always check `err` after `rows.Next()` loop with `rows.Err()`

## Run it

```bash
cd ~/Dev/learn-go/DAY16
go run main.go
```
