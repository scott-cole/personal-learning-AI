# DAY20: Drill — "Add Error Wrapping"

Add error wrapping across all three layers of the URL shortener.

## Structure

```
DAY20/
├── main.go           → wrap errors from service, log them
├── service/
│   └── service.go    → wrap errors from repository
└── storage/
    └── memory.go     → wrap errors with operation context
```

## Step 1: storage/memory.go

```go
func (r *MemoryRepositroy) Save(url storage.URL) error {
    if url.Code == "" {
        return errors.New("code cannot be empty")
    }
    if _, exists := r.data[url.Code]; exists {
        return fmt.Errorf("code %q already exists", url.Code)
    }
    r.data[url.Code] = url
    return nil
}

func (r *MemoryRepositroy) FindByCode(code string) (storage.URL, error) {
    url, ok := r.data[code]
    if !ok {
        return storage.URL{}, fmt.Errorf("code %q not found", code)
    }
    return url, nil
}
```

## Step 2: service/service.go

```go
func (s *Service) Shorten(originalURL string) (storage.URL, error) {
    if !strings.HasPrefix(originalURL, "http://") && !strings.HasPrefix(originalURL, "https://") {
        return storage.URL{}, fmt.Errorf("invalid URL %q: must start with http:// or https://", originalURL)
    }

    code, err := generateCode(8)
    if err != nil {
        return storage.URL{}, fmt.Errorf("generate code: %w", err)
    }

    url := storage.URL{Code: code, OriginalURL: originalURL}
    err = s.repo.Save(url)
    if err != nil {
        return storage.URL{}, fmt.Errorf("save %q: %w", code, err)
    }

    return url, nil
}

func (s *Service) Resolve(code string) (storage.URL, error) {
    url, err := s.repo.FindByCode(code)
    if err != nil {
        return storage.URL{}, fmt.Errorf("resolve %q: %w", code, err)
    }
    return url, nil
}
```

## Step 3: main.go

```go
func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    _, err := svc.Shorten("not-a-url")
    if err != nil {
        log.Printf("Shorten failed: %v", err)
        // Can we check the root cause?
        if strings.Contains(err.Error(), "must start with http") {
            log.Println("→ This was a validation error")
        }
    }

    _, err = svc.Resolve("nonexistent")
    if err != nil {
        log.Printf("Resolve failed: %v", err)
    }

    // Test: can we use errors.Is to check the root cause?
    url := storage.URL{Code: "abc123", OriginalURL: "https://example.com"}
    repo.Save(url)
    _, err = svc.Shorten("https://example.com") // will try to save same code
    if err != nil {
        log.Printf("Shorten duplicate failed: %v", err)
    }
}
```

## Run it

```bash
cd ~/Dev/learn-go/DAY20
go run main.go
```

## Expected output

Look at the log messages. Each one tells you:
- WHAT failed (shorten, resolve)
- WHAT was involved (code, URL)
- WHY it failed (invalid URL, not found, duplicate)

## Your task

Add `fmt.Errorf("...: %w", err)` at every layer boundary in your DAY10 code. Run it and observe the difference in error messages.
