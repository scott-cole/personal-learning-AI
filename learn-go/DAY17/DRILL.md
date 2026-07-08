# DAY17: Drill — "Write Migrations"

Create a migration system for the URL shortener and write the initial migration.

## Structure

```
DAY17/
├── go.mod
├── main.go
└── migrations/
    ├── 001_create_urls.sql
    └── 002_add_access_count.sql
```

## Step 1: 001_create_urls.sql

```sql
CREATE TABLE IF NOT EXISTS urls (
    code         TEXT PRIMARY KEY,
    original_url TEXT NOT NULL,
    created_at   TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS schema_migrations (
    version INTEGER PRIMARY KEY,
    applied_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO schema_migrations (version) VALUES (1);
```

## Step 2: 002_add_access_count.sql

```sql
ALTER TABLE urls ADD COLUMN access_count INTEGER NOT NULL DEFAULT 0;

INSERT INTO schema_migrations (version) VALUES (2);
```

## Step 3: main.go

```go
package main

import (
    "database/sql"
    "fmt"
    "log"
    "os"
    "path/filepath"
    "sort"
    "strings"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    db, err := sql.Open("sqlite3", "./url_shortener.db")
    if err != nil {
        log.Fatal(err)
    }
    defer db.Close()

    // Run migrations
    err = runMigrations(db, "migrations")
    if err != nil {
        log.Fatal(err)
    }
    fmt.Println("Migrations complete!")
}

func runMigrations(db *sql.DB, dir string) error {
    // 1. Read all .sql files from the directory
    files, err := filepath.Glob(filepath.Join(dir, "*.sql"))
    if err != nil {
        return err
    }
    sort.Strings(files) // important: apply in order

    for _, file := range files {
        // 2. Extract version number from filename
        // filename format: "001_name.sql"
        version := filepath.Base(file)[:3]

        // 3. Check if already applied
        var count int
        err := db.QueryRow("SELECT COUNT(*) FROM schema_migrations WHERE version = ?", version).Scan(&count)
        if err != nil {
            return err
        }
        if count > 0 {
            fmt.Printf("Skipping %s (already applied)\n", filepath.Base(file))
            continue
        }

        // 4. Read and execute the migration
        content, err := os.ReadFile(file)
        if err != nil {
            return err
        }

        // Split by semicolons and execute each statement
        statements := strings.Split(string(content), ";")
        for _, stmt := range statements {
            stmt = strings.TrimSpace(stmt)
            if stmt == "" {
                continue
            }
            _, err = db.Exec(stmt)
            if err != nil {
                return fmt.Errorf("migration %s failed: %w", filepath.Base(file), err)
            }
        }

        fmt.Printf("Applied: %s\n", filepath.Base(file))
    }

    return nil
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY17
go mod init learn-go/DAY17
go get github.com/mattn/go-sqlite3
go run main.go
```

Run it twice. The second time should skip migration 001.

## Expected output

First run:
```
Applied: 001_create_urls.sql
Applied: 002_add_access_count.sql
Migrations complete!
```

Second run:
```
Skipping 001_create_urls.sql (already applied)
Skipping 002_add_access_count.sql (already applied)
Migrations complete!
```
