# DAY19: Drill — "Use Transactions"

Add a batch operation to the URL shortener that saves multiple URLs in a single transaction.

## Setup

Copy DAY18 structure into DAY19. Add a new endpoint.

## Step 1: Add batch save to the repository

Add to `storage/repository.go`:

```go
// BulkSave saves multiple URLs in a single transaction
BulkSave(urls []URL) error
```

## Step 2: Implement SQLRepository.BulkSave

In `sqlrepo/sqlite.go`:

```go
func (r *SQLRepository) BulkSave(urls []storage.URL) error {
    tx, err := r.db.Begin()
    if err != nil {
        return err
    }
    defer tx.Rollback()

    stmt, err := tx.Prepare("INSERT INTO urls (code, original_url) VALUES (?, ?)")
    if err != nil {
        return err
    }
    defer stmt.Close()

    for _, url := range urls {
        _, err = stmt.Exec(url.Code, url.OriginalURL)
        if err != nil {
            return err
        }
    }

    return tx.Commit()
}
```

## Step 3: main.go

```go
package main

import (
    "database/sql"
    "fmt"
    "log"
    _ "github.com/mattn/go-sqlite3"
    "learn-go/DAY19/storage"
    "learn-go/DAY19/sqlrepo"
)

func main() {
    db, err := sql.Open("sqlite3", "./url_shortener.db")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // create table...

    repo := sqlrepo.NewSQLRepository(db)

    urls := []storage.URL{
        {Code: "aaa", OriginalURL: "https://example.com/1"},
        {Code: "bbb", OriginalURL: "https://example.com/2"},
        {Code: "ccc", OriginalURL: "https://example.com/3"},
    }

    err = repo.BulkSave(urls)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Saved 3 URLs in one transaction")

    // Verify none were saved if one fails
    err = repo.BulkSave([]storage.URL{
        {Code: "ddd", OriginalURL: "https://example.com/4"},
        {Code: "aaa", OriginalURL: "https://example.com/DUPLICATE"}, // will fail
        {Code: "eee", OriginalURL: "https://example.com/5"},
    })
    if err != nil {
        fmt.Println("Expected error:", err)
    }

    // Verify ddd and eee were NOT saved
    for _, code := range []string{"ddd", "eee"} {
        _, err := repo.FindByCode(code)
        if err != nil {
            fmt.Printf("%s correctly not saved (rolled back)\n", code)
        }
    }

    // Clean up
    db.Exec("DELETE FROM urls")
}
```

## Expected output

```
Saved 3 URLs in one transaction
Expected error: UNIQUE constraint failed: urls.code
ddd correctly not saved (rolled back)
eee correctly not saved (rolled back)
```

## Run it

```bash
cd ~/Dev/learn-go/DAY19
go mod init learn-go/DAY19
go get github.com/mattn/go-sqlite3
go run main.go
```
