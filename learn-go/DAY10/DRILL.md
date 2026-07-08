# DAY10: Drill — "The URL Shortener Service"

Build the service layer that sits on top of your repository.

## Structure

```
DAY10/
├── go.mod
├── main.go
├── storage/
│   └── memory.go        → copy from DAY09
└── service/
    └── service.go        → new file
```

### Step 1: Setup

Copy your `DAY09` structure. The `go.mod` module is `learn-go/DAY10`.

### Step 2: service/service.go

```go
package service

import (
    "crypto/rand"
    "errors"
    "math/big"
    "strings"
    "learn-go/DAY10/storage"
)

const charset = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789"

type Service struct {
    repo storage.Repository
}

func NewService(repo storage.Repository) *Service {
    return &Service{repo: repo}
}

// Shorten generates a short code and saves the URL
func (s *Service) Shorten(originalURL string) (storage.URL, error) {
    if !strings.HasPrefix(originalURL, "http://") && !strings.HasPrefix(originalURL, "https://") {
        return storage.URL{}, errors.New("url must start with http:// or https://")
    }

    code, err := generateCode(8)
    if err != nil {
        return storage.URL{}, err
    }

    url := storage.URL{
        Code:        code,
        OriginalURL: originalURL,
    }

    err = s.repo.Save(url)
    if err != nil {
        return storage.URL{}, err
    }

    return url, nil
}

// Resolve looks up a code and returns the original URL
func (s *Service) Resolve(code string) (storage.URL, error) {
    return s.repo.FindByCode(code)
}

// generateCode creates a random string of the given length
func generateCode(length int) (string, error) {
    result := make([]byte, length)
    for i := range result {
        n, err := rand.Int(rand.Reader, big.NewInt(int64(len(charset))))
        if err != nil {
            return "", err
        }
        result[i] = charset[n.Int64()]
    }
    return string(result), nil
}
```

### Step 3: main.go

```go
package main

import (
    "fmt"
    "learn-go/DAY10/service"
    "learn-go/DAY10/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    // Shorten a URL
    result, err := svc.Shorten("https://www.example.com/very/long/url")
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Printf("Shortened: %s → %s\n", result.Code, result.OriginalURL)

    // Resolve it
    resolved, err := svc.Resolve(result.Code)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    fmt.Printf("Resolved: %s → %s\n", resolved.Code, resolved.OriginalURL)

    // Test invalid URL
    _, err = svc.Shorten("not-a-url")
    if err != nil {
        fmt.Println("Expected error:", err)
    }

    // Test not found
    _, err = svc.Resolve("nonexistent")
    if err != nil {
        fmt.Println("Expected error:", err)
    }
}
```

## Expected output

```
Shortened: aB3xK9mQ → https://www.example.com/very/long/url
Resolved: aB3xK9mQ → https://www.example.com/very/long/url
Expected error: url must start with http:// or https://
Expected error: not found
```

The code will be different every time (random generation).

## Run it

```bash
cd ~/Dev/learn-go/DAY10
go run main.go
```
