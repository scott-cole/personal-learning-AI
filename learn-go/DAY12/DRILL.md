# DAY12: Drill — "JSON Handlers for the URL Shortener"

Upgrade your URL shortener handlers to speak proper JSON.

## Structure

```
DAY12/
├── go.mod
├── main.go              → JSON handlers
├── storage/
│   └── memory.go
└── service/
    └── service.go
```

### Request and response types

Add these types at the top of `main.go`:

```go
type shortenRequest struct {
    URL string `json:"url"`
}

type shortenResponse struct {
    Code        string `json:"code"`
    OriginalURL string `json:"original_url"`
    ShortURL    string `json:"short_url"`
}

type errorResponse struct {
    Error string `json:"error"`
}
```

### JSON handler for POST /shorten

```go
mux.HandleFunc("POST /shorten", func(w http.ResponseWriter, r *http.Request) {
    var req shortenRequest
    err := json.NewDecoder(r.Body).Decode(&req)
    if err != nil {
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusBadRequest)
        json.NewEncoder(w).Encode(errorResponse{Error: "invalid JSON"})
        return
    }

    result, err := svc.Shorten(req.URL)
    if err != nil {
        w.Header().Set("Content-Type", "application/json")
        w.WriteHeader(http.StatusBadRequest)
        json.NewEncoder(w).Encode(errorResponse{Error: err.Error()})
        return
    }

    resp := shortenResponse{
        Code:        result.Code,
        OriginalURL: result.OriginalURL,
        ShortURL:    "http://localhost:8080/" + result.Code,
    }

    w.Header().Set("Content-Type", "application/json")
    w.WriteHeader(http.StatusCreated)  // 201
    json.NewEncoder(w).Encode(resp)
})
```

### Test with curl

```bash
cd ~/Dev/learn-go/DAY12
go run main.go
```

In another terminal:

```bash
# Shorten a URL — get back JSON
curl -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/very/long/url"}'

# Expected:
# {"code":"aB3xK9mQ","original_url":"https://example.com/very/long/url","short_url":"http://localhost:8080/aB3xK9mQ"}

# Resolve the short URL
curl -v http://localhost:8080/aB3xK9mQ   # ← redirects to the original

# Test invalid input
curl -X POST http://localhost:8080/shorten \
  -H "Content-Type: application/json" \
  -d 'invalid json'

# Expected:
# {"error":"invalid JSON"}
```

## Don't forget

- Add `"encoding/json"` to your imports
- The `GET /{code}` redirect handler stays the same from DAY11
- The old `POST /shorten` that read plain text should be replaced

## Run it

```bash
cd ~/Dev/learn-go/DAY12
go run main.go
```
