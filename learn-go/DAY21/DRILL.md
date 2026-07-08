# DAY21: Drill — "Week 3 Mini-Project: Stats CLI"

Build the CLI described in README.md.

## Starter main.go

```go
package main

import (
    "database/sql"
    "flag"
    "fmt"
    "log"
    "os"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    dbPath := flag.String("db", "./url_shortener.db", "path to SQLite database")
    insertURL := flag.String("insert", "", "URL to insert")
    flag.Parse()

    // Check if database exists
    if _, err := os.Stat(*dbPath); os.IsNotExist(err) {
        log.Fatalf("Database %s not found", *dbPath)
    }

    db, err := sql.Open("sqlite3", *dbPath)
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    if *insertURL != "" {
        // TODO: Insert the URL with a random code
        // Use crypto/rand to generate an 8-char code
        // INSERT INTO urls (code, original_url) VALUES (?, ?)
        fmt.Printf("Inserted: %s\n", *insertURL)
        return
    }

    // TODO: Query all URLs and print stats
    // SELECT code, original_url, access_count FROM urls
    // Print formatted output
}
```

## Hints

- Use `os.Stat` + `os.IsNotExist` to check if the DB file exists
- Use `fmt.Printf` for formatted output
- For the insert flag, reuse the `generateCode` function from DAY10

## Expected output

```
All URLs:
abc123 → https://example.com (5 visits)
def456 → https://google.com (3 visits)

Total URLs: 2
Total visits: 8
```

## Run it

```bash
cd ~/Dev/learn-go/DAY21
go run main.go
go run main.go -insert "https://github.com"
go run main.go
```
