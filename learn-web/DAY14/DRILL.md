# DAY14: Drill — "Trigger a CORS error and fix it"

Observe CORS in action, trigger an error, and simulate a fix.

## Part 1 — See CORS headers in curl

```bash
# Fetch a public API and examine its CORS headers
curl -I https://api.github.com/users/scott
# Look for: access-control-allow-origin

# Some APIs are more explicit
curl -I https://jsonplaceholder.typicode.com/posts/1
# Note: Not all APIs set CORS headers — they don't need to for public read access
```

## Part 2 — Trigger a CORS error in the browser

1. Open your browser's DevTools console (Cmd+Option+I)
2. Type this and press Enter:

```javascript
fetch('https://jsonplaceholder.typicode.com/posts/1', {
  method: 'DELETE'
})
.then(r => r.json())
.then(console.log)
.catch(console.error)
```

Take a screenshot of the error message.

## Part 3 — Simulate CORS with a local Go server

Create `server.go`:

```go
package main

import (
    "fmt"
    "net/http"
)

func cors(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Access-Control-Allow-Origin", "*")
        w.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
        w.Header().Set("Access-Control-Allow-Headers", "Content-Type")
        if r.Method == "OPTIONS" {
            w.WriteHeader(http.StatusNoContent)
            return
        }
        next.ServeHTTP(w, r)
    })
}

func handler(w http.ResponseWriter, r *http.Request) {
    w.Header().Set("Content-Type", "application/json")
    fmt.Fprintf(w, `{"message":"CORS is working!"}`)
}

func main() {
    http.Handle("/", cors(http.HandlerFunc(handler)))
    http.ListenAndServe(":8080", nil)
}
```

Run it: `go run server.go`

In another terminal, test:

```bash
curl -v http://localhost:8080
# Look for: access-control-allow-origin: *
```

Now try from the browser console:

```javascript
fetch('http://localhost:8080')
  .then(r => r.json())
  .then(console.log)
```

## Part 4 — Trigger a preflight

```bash
curl -v -X PUT -H "Content-Type: application/json" http://localhost:8080
# You should see:
# access-control-allow-methods: GET, POST, PUT, DELETE, OPTIONS
```

## Part 5 — Remove CORS and see the error

Comment out the CORS middleware and restart the server. Now try the fetch from the browser again. What error do you see?

## Deliverables

1. The CORS error message from Part 2
2. The output from your running server (with and without CORS)
3. Did the browser fetch work without CORS headers?

## Hints

<details>
<summary>Click for hints</summary>

- CORS errors only appear in the browser, never in curl
- Check the **Console** and **Network** tabs in DevTools
- The preflight OPTIONS request won't appear in curl unless you explicitly send it
- `*` origin allows any website — but can't be used with credentials (cookies, auth headers)
</details>
