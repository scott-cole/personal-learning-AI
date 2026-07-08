# DAY08: Build a Raw HTTP Client in Go

---

## The anecdote: "Building an HTTP client is like wiring a telephone yourself"

Yesterday you used a telephone (curl). Today you'll build one from scratch using wires and circuits.

Go's `net/http` package gives you all the parts. You just need to connect them:

```
User code → http.Client → http.Request → TCP connection → Server
                                                   ↓
User code ← http.Response ← read body ←─────────────
```

---

## The minimal client

```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    resp, err := http.Get("https://api.github.com/users/scott")
    if err != nil {
        fmt.Fprintf(os.Stderr, "Error: %v\n", err)
        os.Exit(1)
    }
    defer resp.Body.Close()

    body, err := io.ReadAll(resp.Body)
    if err != nil {
        fmt.Fprintf(os.Stderr, "Error reading body: %v\n", err)
        os.Exit(1)
    }

    fmt.Printf("Status: %s\n", resp.Status)
    fmt.Printf("Body:  %s\n", body)
}
```

Run it:

```bash
go run main.go
```

---

## The `http.Client` type

`http.Get` uses a default client. For real work, create your own:

```go
client := &http.Client{
    Timeout: 10 * time.Second,
    // Don't follow redirects automatically
    CheckRedirect: func(req *http.Request, via []*http.Request) error {
        return http.ErrUseLastResponse
    },
}

resp, err := client.Get("https://api.github.com/users/scott")
```

---

## Building a full request manually

```go
package main

import (
    "bytes"
    "encoding/json"
    "fmt"
    "net/http"
    "time"
)

type Payload struct {
    Title  string `json:"title"`
    Body   string `json:"body"`
    UserID int    `json:"userId"`
}

func main() {
    payload := Payload{Title: "foo", Body: "bar", UserID: 1}
    jsonBytes, _ := json.Marshal(payload)

    req, _ := http.NewRequest(
        "POST",
        "https://jsonplaceholder.typicode.com/posts",
        bytes.NewReader(jsonBytes),
    )
    req.Header.Set("Content-Type", "application/json")
    req.Header.Set("User-Agent", "learn-web-client/1.0")

    client := &http.Client{Timeout: 10 * time.Second}
    resp, err := client.Do(req)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer resp.Body.Close()

    fmt.Printf("Status: %s\n", resp.Status)
    for k, v := range resp.Header {
        fmt.Printf("%s: %s\n", k, v)
    }
}
```

---

## Reading the response the right way

```go
// Method 1: Read all (for small bodies ≤ 10MB)
body, _ := io.ReadAll(resp.Body)

// Method 2: Stream as JSON (for large responses)
var result map[string]interface{}
json.NewDecoder(resp.Body).Decode(&result)

// Method 3: Copy to a file (for downloads)
f, _ := os.Create("download.zip")
io.Copy(f, resp.Body)
f.Close()
```

---

## Error handling checklist

```go
resp, err := client.Do(req)
if err != nil {
    // Network error, timeout, DNS failure
    // DO NOT use resp here — it's nil
    return
}
defer resp.Body.Close()

// Check status code
if resp.StatusCode < 200 || resp.StatusCode >= 300 {
    body, _ := io.ReadAll(resp.Body)
    return fmt.Errorf("HTTP %d: %s", resp.StatusCode, body)
}
```

---

## Senior mindset: "Default client is for examples only"

The default HTTP client has **no timeout**. If a server hangs, your program hangs forever.

```go
// DANGER: no timeout
resp, _ := http.Get("https://example.com")

// SAFE: 10 second timeout
client := &http.Client{Timeout: 10 * time.Second}
resp, _ := client.Get("https://example.com")
```

Always create a client with a timeout. Always `defer resp.Body.Close()`. Always check errors.

---

## Summary

| Step | Code |
|------|------|
| Simple GET | `http.Get(url)` |
| Custom client | `&http.Client{Timeout: 10 * time.Second}` |
| Full request | `http.NewRequest(method, url, body)` |
| Send request | `client.Do(req)` |
| Read body | `io.ReadAll(resp.Body)` |
| Stream JSON | `json.NewDecoder(resp.Body).Decode(&v)` |
