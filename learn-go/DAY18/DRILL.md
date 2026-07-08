# DAY18: Drill — "Write the SQL Repository"

Create a `SQLRepository` that implements the same `Repository` interface as `MemoryRepository`.

## Structure

```
DAY18/
├── go.mod
├── main.go
├── storage/
│   ├── memory.go         → implemented as before
│   └── repository.go     → shared Repository interface
└── sqlrepo/
    └── sqlite.go         → SQLRepository implementation
```

## Step 1: storage/repository.go

```go
package storage

import "time"

type URL struct {
    Code         string
    OriginalURL  string
    CreatedAt    time.Time
    AccessCount  int
}

type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
}
```

## Step 2: sqlrepo/sqlite.go

```go
package sqlrepo

import (
    "database/sql"
    "errors"
    "time"
    "learn-go/DAY18/storage"
)

type SQLRepository struct {
    db *sql.DB
}

func NewSQLRepository(db *sql.DB) *SQLRepository {
    return &SQLRepository{db: db}
}

func (r *SQLRepository) Save(url storage.URL) error {
    // TODO: INSERT INTO urls (code, original_url, created_at) VALUES (?, ?, ?)
    // Return error if code already exists
}

func (r *SQLRepository) FindByCode(code string) (storage.URL, error) {
    // TODO: SELECT code, original_url, access_count, created_at FROM urls WHERE code = ?
    // Map sql.ErrNoRows to "not found"
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
    "learn-go/DAY18/storage"
    "learn-go/DAY18/sqlrepo"
)

func main() {
    db, err := sql.Open("sqlite3", "./url_shortener.db")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // Create table
    _, err = db.Exec(`CREATE TABLE IF NOT EXISTS urls (
        code TEXT PRIMARY KEY,
        original_url TEXT NOT NULL,
        access_count INTEGER DEFAULT 0,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
    )`)
    if err != nil {
        log.Fatal(err)
    }

    // Use the SQL repository via the interface
    var repo storage.Repository = sqlrepo.NewSQLRepository(db)

    // Test it
    url := storage.URL{Code: "abc123", OriginalURL: "https://example.com"}
    err = repo.Save(url)
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Saved:", url.Code)

    found, err := repo.FindByCode("abc123")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Printf("Found: %s -> %s\n", found.Code, found.OriginalURL)

    // Clean up
    db.Exec("DELETE FROM urls")
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY18
go mod init learn-go/DAY18
go get github.com/mattn/go-sqlite3
go run main.go
```

## Expected output

```
Saved: abc123
Found: abc123 -> https://example.com
```
