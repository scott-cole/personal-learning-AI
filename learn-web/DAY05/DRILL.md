# DAY05: Drill — "Ship some packages"

Use curl to send and receive different body types.

## Part 1 — Send JSON

```bash
curl -v -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -d '{"product":"laptop","price":999.99,"quantity":1}'
```

httpbin.org echoes back what you sent. Look at the `json` field in the response.

## Part 2 — Send a file as the body

```bash
# Create a simple JSON file
echo '{"hello":"world"}' > /tmp/test.json

# Send it
curl -v -X POST https://httpbin.org/post \
  -H "Content-Type: application/json" \
  -d @/tmp/test.json
```

The `@` prefix tells curl to read the body from a file.

## Part 3 — Form data

```bash
curl -v -X POST https://httpbin.org/post \
  -d "search=golang&page=2&sort=stars"
```

Notice how the `Content-Type` changes (curl auto-detects it).

## Part 4 — Send binary data

```bash
# Download an image
curl -s https://httpbin.org/image/png -o /tmp/image.png

# Upload it
curl -v -X POST https://httpbin.org/post \
  -F "image=@/tmp/image.png"
```

## Part 5 — Read response body in Go

Create `main.go`:

```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
)

func main() {
    if len(os.Args) < 2 {
        fmt.Println("Usage: go run main.go <url>")
        os.Exit(1)
    }

    resp, err := http.Get(os.Args[1])
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(1)
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)
    fmt.Printf("Status: %s\n", resp.Status)
    fmt.Printf("Body:\n%s\n", body)
}
```

Run it:

```bash
go run main.go https://example.com
```

## Deliverables

Show me your Go program working with at least 3 different URLs.

## Hints

<details>
<summary>Click for hints</summary>

- `@` reads from a file: `-d @file.json`
- `-F` automatically sets `multipart/form-data`
- httpbin.org's `/post` endpoint echoes whatever you POST
- Always `defer resp.Body.Close()` in Go
</details>
