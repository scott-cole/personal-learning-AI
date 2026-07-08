# DAY23: Testing — "Tests Are Not Optional"

---

## The anecdote: "A safety net"

A tightrope walker doesn't practice without a net. You don't refactor or deploy without tests.

Tests are your safety net. They tell you:
- "This change broke nothing" (green)
- "This change broke something specific" (red + line number)

Without tests, every deployment is a leap of faith.

---

## Go's testing package

Every `_test.go` file is a test:

```go
// main_test.go
package main

import "testing"

func TestAdd(t *testing.T) {
    result := add(2, 3)
    expected := 5
    if result != expected {
        t.Errorf("add(2, 3) = %d; want %d", result, expected)
    }
}
```

Run: `go test .`

---

## Table-driven tests

The Go standard pattern:

```go
func TestShorten(t *testing.T) {
    tests := []struct {
        name    string
        url     string
        wantErr bool
    }{
        {name: "valid https", url: "https://example.com", wantErr: false},
        {name: "valid http", url: "http://example.com", wantErr: false},
        {name: "no protocol", url: "not-a-url", wantErr: true},
        {name: "empty string", url: "", wantErr: true},
    }

    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            _, err := svc.Shorten(tt.url)
            if (err != nil) != tt.wantErr {
                t.Errorf("Shorten(%q) error = %v, wantErr = %v", tt.url, err, tt.wantErr)
            }
        })
    }
}
```

---

## Testing each layer

### Repository — test with real memory store

```go
func TestMemoryRepository_Save(t *testing.T) {
    repo := storage.NewMemoryRepository()
    err := repo.Save(storage.URL{Code: "abc", OriginalURL: "https://example.com"})
    if err != nil {
        t.Fatal(err)
    }
    // Verify it was saved
    url, err := repo.FindByCode("abc")
    if err != nil {
        t.Fatal(err)
    }
    if url.OriginalURL != "https://example.com" {
        t.Errorf("got %q, want %q", url.OriginalURL, "https://example.com")
    }
}
```

### Service — test with mock repository

```go
type mockRepo struct {
    storage.Repository
    saveFunc func(storage.URL) error
}

func (m *mockRepo) Save(url storage.URL) error {
    return m.saveFunc(url)
}

func TestService_Shorten_ValidatesURL(t *testing.T) {
    repo := &mockRepo{
        saveFunc: func(url storage.URL) error { return nil },
    }
    svc := service.NewService(repo)

    _, err := svc.Shorten("not-a-url")
    if err == nil {
        t.Error("expected error for invalid URL")
    }
}
```

### Handler — test with httptest

```go
func TestShortenHandler(t *testing.T) {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // your handler code
    })

    body := strings.NewReader(`{"url":"https://example.com"}`)
    req := httptest.NewRequest("POST", "/shorten", body)
    req.Header.Set("Content-Type", "application/json")
    w := httptest.NewRecorder()

    handler.ServeHTTP(w, req)

    if w.Code != http.StatusCreated {
        t.Errorf("got status %d, want %d", w.Code, http.StatusCreated)
    }
}
```

---

## Test coverage

```bash
go test -cover .
```

Shows what percentage of code is covered. Aim for 80%+ on service and repository (business logic). Handlers are harder to cover but still valuable.

---

## Senior mindset: "Test behaviour, not implementation"

```go
// ❌ Don't test that Save was called
// ✅ Test that the URL is retrievable after shortening
```

Your tests should pass even if you completely rewrite the internals. Test what the code **does**, not how it does it.

---

## Summary

| Pattern | Use |
|---------|-----|
| `TestXxx(t *testing.T)` | Basic test function |
| Table-driven tests | Multiple cases, same logic |
| `httptest.NewRecorder()` | Test HTTP handlers |
| `go test -cover` | Check coverage |
| `t.Run(name, fn)` | Sub-tests (better error output) |

Open DAY23/DRILL.md.
