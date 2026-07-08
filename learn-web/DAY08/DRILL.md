# DAY08: Drill — "Build a multi-tool HTTP client"

Write a Go program called `main.go` that is a flexible HTTP client.

## Requirements

Your program must:
1. Accept a URL as a command-line argument
2. Accept flags:
   - `-X` or `--method` (default: GET)
   - `-d` or `--data` (request body)
   - `-H` or `--header` (repeatable: `-H "Key: Value"`)
   - `-t` or `--timeout` (in seconds, default: 10)
3. Print: status code, all response headers, and the body
4. Handle errors gracefully

## Starter code

```go
package main

import (
    "flag"
    "fmt"
    "io"
    "net/http"
    "os"
    "strings"
    "time"
)

func main() {
    method := flag.String("X", "GET", "HTTP method")
    data := flag.String("d", "", "Request body")
    timeout := flag.Int("t", 10, "Timeout in seconds")
    flag.Parse()

    if flag.NArg() < 1 {
        fmt.Println("Usage: go run main.go [-X method] [-d body] [-H header] [-t timeout] <url>")
        os.Exit(1)
    }

    url := flag.Arg(0)

    // Build request body
    var body io.Reader
    if *data != "" {
        body = strings.NewReader(*data)
    }

    req, err := http.NewRequest(*method, url, body)
    if err != nil {
        fmt.Fprintf(os.Stderr, "Error creating request: %v\n", err)
        os.Exit(1)
    }

    // TODO: Parse custom headers and add them to req

    client := &http.Client{Timeout: time.Duration(*timeout) * time.Second}
    resp, err := client.Do(req)
    // TODO: Handle errors
    // TODO: Print status, headers, body
}
```

## Fill in the TODOs

1. Parse multiple `-H` flags (use a custom flag or var)
2. Add error handling for the request
3. Print the status code, all headers, and the body
4. Close the response body

## Example usage

```bash
go run main.go https://api.github.com/users/scott

go run main.go -X POST -d '{"title":"test"}' \
  -H "Content-Type: application/json" \
  https://jsonplaceholder.typicode.com/posts

go run main.go -t 5 https://example.com
```

## Bonus

Add `-v` verbose mode that prints the request details before sending.

## Hints

<details>
<summary>Click for hints</summary>

- Use `flag.Func` or `flag.Var` with a custom type for repeatable `-H` flags
- `resp.Header` is a `map[string][]string` — iterate with `for k, v := range resp.Header`
- After printing the body, the program should exit cleanly (no panic, no extra newlines)
- `io.ReadAll(resp.Body)` returns `[]byte`, convert with `string(body)`
</details>
