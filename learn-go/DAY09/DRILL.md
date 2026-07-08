# DAY09: Drill — "Organise the Repository"

Take your code from DAY08 and split it into proper packages.

## Requirements

Create this structure:

```
DAY09/
├── main.go
├── go.mod
└── storage/
    └── memory.go
```

### Step 1: go.mod

```
cd DAY09
go mod init learn-go/DAY09
```

### Step 2: storage/memory.go

```go
package storage

import (
    "errors"
    "time"
)

// URL is the data model
type URL struct {
    Code        string
    OriginalURL string
    CreatedAt   time.Time
}

// Repository defines the data access interface
type Repository interface {
    Save(url URL) error
    FindByCode(code string) (URL, error)
}

// MemoryRepository stores URLs in memory
type MemoryRepository struct {
    data map[string]URL
}

func NewMemoryRepository() *MemoryRepository {
    return &MemoryRepository{
        data: make(map[string]URL),
    }
}

func (r *MemoryRepository) Save(url URL) error {
    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepository) FindByCode(code string) (URL, error) {
    url, ok := r.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}
```

### Step 3: main.go

```go
package main

import (
    "fmt"
    "time"
    "learn-go/DAY09/storage"     // import your package
)

func main() {
    repo := storage.NewMemoryRepository()

    url := storage.URL{
        Code:        "abc123",
        OriginalURL: "https://example.com",
        CreatedAt:   time.Now(),
    }

    err := repo.Save(url)
    if err != nil {
        fmt.Println("Save error:", err)
        return
    }
    fmt.Println("Saved:", url.Code)

    found, err := repo.FindByCode("abc123")
    if err != nil {
        fmt.Println("Find error:", err)
        return
    }
    fmt.Printf("Found: %s → %s\n", found.Code, found.OriginalURL)

    _, err = repo.FindByCode("nonexistent")
    if err != nil {
        fmt.Println("Expected error:", err)
    }
}
```

## Expected output

```
Saved: abc123
Found: abc123 → https://example.com
Expected error: not found
```

## What you just built

A **repository layer** — the bottom of the backend stack. Tomorrow you'll build the **service layer** on top of it.

## Run it

```bash
cd ~/Dev/learn-go/DAY09
go run main.go
```
