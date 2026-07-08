# DAY24: Drill — "Make the Repository Concurrent-Safe"

Add mutex protection to the MemoryRepository and test it with concurrent access.

## Structure

```
DAY24/
├── main.go              → concurrent access demo
└── storage/
    └── memory.go        → add sync.RWMutex
```

## Step 1: Update storage/memory.go

Add `sync.RWMutex` to MemoryRepository and protect all methods:

```go
package storage

import (
    "errors"
    "sync"
    "time"
)

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

type MemoryRepositroy struct {
    mu   sync.RWMutex
    data map[string]URL
}

func NewMemoryRepository() *MemoryRepositroy {
    return &MemoryRepositroy{
        data: make(map[string]URL),
    }
}

func (r *MemoryRepositroy) Save(url URL) error {
    r.mu.Lock()
    defer r.mu.Unlock()

    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepositroy) FindByCode(code string) (URL, error) {
    r.mu.RLock()
    defer r.mu.RUnlock()

    url, ok := r.data[code]
    if !ok {
        return URL{}, errors.New("not found")
    }
    return url, nil
}
```

## Step 2: Test with concurrent access

```go
package main

import (
    "fmt"
    "sync"
    "learn-go/DAY24/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    var wg sync.WaitGroup

    // Launch 10 goroutines that save URLs concurrently
    for i := 0; i < 10; i++ {
        wg.Add(1)
        go func(n int) {
            defer wg.Done()
            code := fmt.Sprintf("code-%d", n)
            url := storage.URL{
                Code:        code,
                OriginalURL: fmt.Sprintf("https://example.com/%d", n),
            }
            err := repo.Save(url)
            if err != nil {
                fmt.Printf("Save error: %v\n", err)
                return
            }
            fmt.Printf("Saved: %s\n", code)
        }(i)
    }

    // Launch 10 goroutines that read URLs concurrently
    for i := 0; i < 10; i++ {
        wg.Add(1)
        go func(n int) {
            defer wg.Done()
            code := fmt.Sprintf("code-%d", n)
            url, err := repo.FindByCode(code)
            if err != nil {
                fmt.Printf("Find error for %s: %v\n", code, err)
                return
            }
            fmt.Printf("Found: %s -> %s\n", url.Code, url.OriginalURL)
        }(i)
    }

    wg.Wait()
    fmt.Println("All done!")
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY24
go mod init learn-go/DAY24
go run main.go
```

## Expected

All 10 saves and 10 finds complete without a panic. The output will be interleaved.

## Verify it works

Remove the `mu.Lock()` / `mu.RUnlock()` calls and run again. You'll get:
```
fatal error: concurrent map writes
```

That's Go protecting you from yourself. The mutex is your safety.

## Run it

```bash
cd ~/Dev/learn-go/DAY24
go run main.go
```
