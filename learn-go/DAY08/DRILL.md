# DAY08: Drill — "The URL Shortener Storage Interface"

Today you're starting the URL shortener. You'll define the `Storage` interface and implement it with an in-memory store.

## Requirements

Create a `main.go` in `DAY08/`:

```go
package main

import (
    "errors"
    "fmt"
)

// 1. Define a URL struct with:
//    - Code (string) — the short code like "abc123"
//    - OriginalURL (string) — the full URL like "https://example.com/page"
//    - CreatedAt (time.Time)
type URL struct {
    Code        string
    OriginalURL string
}

// 2. Define a Storage interface with:
//    - Save(url URL) error
//    - FindByCode(code string) (URL, error)
type Storage interface {
    // TODO
}

// 3. Implement Storage with memoryStore
type memoryStore struct {
    // TODO: add a map[string]URL field
}

func NewMemoryStore() *memoryStore {
    // TODO: return a new memoryStore with an initialised map
}

func (m *memoryStore) Save(url URL) error {
    // TODO: save url to the map by url.Code
    // If url.Code is empty, return an error
}

func (m *memoryStore) FindByCode(code string) (URL, error) {
    // TODO: look up by code, return error if not found
}

// 4. Test it in main
func main() {
    store := NewMemoryStore()

    url := URL{Code: "abc123", OriginalURL: "https://example.com"}
    err := store.Save(url)
    if err != nil {
        fmt.Println("Save error:", err)
        return
    }
    fmt.Println("Saved:", url.Code)

    found, err := store.FindByCode("abc123")
    if err != nil {
        fmt.Println("Find error:", err)
        return
    }
    fmt.Printf("Found: %s → %s\n", found.Code, found.OriginalURL)

    // Test not found
    _, err = store.FindByCode("nonexistent")
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

## Hints

- Use `make(map[string]URL)` to create the map
- `Save` should check if `url.Code == ""` and return an error
- `FindByCode` returns `URL{}, errors.New("not found")` if key doesn't exist

## Run it

```bash
cd ~/Dev/learn-go/DAY08
go run main.go
```
