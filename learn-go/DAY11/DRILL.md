# DAY11: Drill — "HTTP Handlers for the URL Shortener"

Add HTTP handlers that expose your service over the web.

## Structure

```
DAY11/
├── go.mod
├── main.go              → HTTP server + handlers
├── storage/
│   └── memory.go        → copy from DAY10
└── service/
    └── service.go       → copy from DAY10
```

### main.go

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "learn-go/DAY11/service"
    "learn-go/DAY11/storage"
)

func main() {
    repo := storage.NewMemoryRepository()
    svc := service.NewService(repo)

    mux := http.NewServeMux()
    mux.HandleFunc("GET /", homeHandler)
    mux.HandleFunc("GET /{code}", func(w http.ResponseWriter, r *http.Request) {
        // TODO: get code from r.PathValue("code")
        // TODO: call svc.Resolve(code)
        // TODO: if error, return 404
        // TODO: redirect to the original URL
    })
    mux.HandleFunc("POST /shorten", func(w http.ResponseWriter, r *http.Request) {
        // TODO: read body with io.ReadAll(r.Body)
        // TODO: parse as JSON (for now, just read the raw string)
        // TODO: call svc.Shorten(url)
        // TODO: write the short code as response
    })

    fmt.Println("Server starting on :8080")
    log.Fatal(http.ListenAndServe(":8080", mux))
}

func homeHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "URL Shortener API\n")
    fmt.Fprintf(w, "POST /shorten  - Shorten a URL\n")
    fmt.Fprintf(w, "GET /{code}    - Resolve a short code\n")
}
```

## Your task

Fill in the two TODO handlers.

### GET /{code} handler

```go
code := r.PathValue("code")
url, err := svc.Resolve(code)
if err != nil {
    http.Error(w, "not found", http.StatusNotFound)
    return
}
http.Redirect(w, r, url.OriginalURL, http.StatusFound)
```

### POST /shorten handler

```go
body, err := io.ReadAll(r.Body)
if err != nil {
    http.Error(w, "bad request", http.StatusBadRequest)
    return
}
originalURL := string(body)
result, err := svc.Shorten(originalURL)
if err != nil {
    http.Error(w, err.Error(), http.StatusBadRequest)
    return
}
fmt.Fprintf(w, "http://localhost:8080/%s\n", result.Code)
```

## Test it

```bash
cd ~/Dev/learn-go/DAY11
go run main.go
```

Then in another terminal:

```bash
# Shorten a URL
curl -X POST http://localhost:8080/shorten -d "https://example.com/very/long/url"

# Resolve it (replace CODE with what you got)
curl -v http://localhost:8080/CODE
```

The `-v` flag shows the redirect response.
