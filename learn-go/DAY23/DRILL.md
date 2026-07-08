# DAY23: Drill — "Write Tests"

Write tests for all three layers of the URL shortener.

## Structure

```
DAY23/
├── main.go
├── main_test.go         → handler tests
├── service/
│   ├── service.go
│   └── service_test.go  → service layer tests
└── storage/
    ├── memory.go
    └── memory_test.go   → repository tests
```

## Task 1: storage/memory_test.go

```go
package storage

import "testing"

func TestMemoryRepository_SaveAndFind(t *testing.T) {
    repo := NewMemoryRepository()

    url := URL{Code: "test", OriginalURL: "https://test.com"}
    err := repo.Save(url)
    if err != nil {
        t.Fatal(err)
    }

    found, err := repo.FindByCode("test")
    if err != nil {
        t.Fatal(err)
    }
    if found.OriginalURL != "https://test.com" {
        t.Errorf("got %q, want %q", found.OriginalURL, "https://test.com")
    }
}

func TestMemoryRepository_FindNotFound(t *testing.T) {
    repo := NewMemoryRepository()
    _, err := repo.FindByCode("nonexistent")
    if err == nil {
        t.Error("expected error for nonexistent code")
    }
}

func TestMemoryRepository_EmptyCode(t *testing.T) {
    repo := NewMemoryRepository()
    err := repo.Save(URL{Code: "", OriginalURL: "https://test.com"})
    if err == nil {
        t.Error("expected error for empty code")
    }
}
```

## Task 2: service/service_test.go

```go
package service

import (
    "testing"
    "learn-go/DAY23/storage"
)

type mockRepo struct {
    saveFunc func(storage.URL) error
}

func (m *mockRepo) Save(url storage.URL) error {
    return m.saveFunc(url)
}

func (m *mockRepo) FindByCode(code string) (storage.URL, error) {
    return storage.URL{}, nil
}

func TestShorten_ValidURL(t *testing.T) {
    repo := &mockRepo{
        saveFunc: func(url storage.URL) error { return nil },
    }
    svc := NewService(repo)

    result, err := svc.Shorten("https://example.com")
    if err != nil {
        t.Fatal(err)
    }
    if result.Code == "" {
        t.Error("expected non-empty code")
    }
    if result.OriginalURL != "https://example.com" {
        t.Errorf("got %q, want %q", result.OriginalURL, "https://example.com")
    }
}

func TestShorten_InvalidURL(t *testing.T) {
    repo := &mockRepo{
        saveFunc: func(url storage.URL) error { return nil },
    }
    svc := NewService(repo)

    _, err := svc.Shorten("not-a-url")
    if err == nil {
        t.Error("expected error for invalid URL")
    }
}
```

## Task 3: main_test.go

```go
package main

import (
    "encoding/json"
    "net/http"
    "net/http/httptest"
    "strings"
    "testing"
    "learn-go/DAY23/service"
    "learn-go/DAY23/storage"
)

func TestShortenHandler(t *testing.T) {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    mux := http.NewServeMux()
    mux.HandleFunc("POST /shorten", func(w http.ResponseWriter, r *http.Request) {
        var req struct {
            URL string `json:"url"`
        }
        json.NewDecoder(r.Body).Decode(&req)
        result, err := svc.Shorten(req.URL)
        if err != nil {
            http.Error(w, err.Error(), http.StatusBadRequest)
            return
        }
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusCreated)
        json.NewEncoder(w).Encode(result)
    })

    body := `{"url":"https://example.com"}`
    req := httptest.NewRequest("POST", "/shorten", strings.NewReader(body))
    req.Header.Set("Content-Type", "application/json")
    w := httptest.NewRecorder()

    mux.ServeHTTP(w, req)

    if w.Code != http.StatusCreated {
        t.Errorf("got status %d, want %d", w.Code, http.StatusCreated)
    }

    var resp storage.URL
    err := json.Unmarshal(w.Body.Bytes(), &resp)
    if err != nil {
        t.Fatal(err)
    }
    if resp.Code == "" {
        t.Error("expected non-empty code in response")
    }
}
```

## Run tests

```bash
cd ~/Dev/learn-go/DAY23
go mod init learn-go/DAY23
go test -v ./...
```

## Expected

All tests pass with `--- PASS:` for each test case.
