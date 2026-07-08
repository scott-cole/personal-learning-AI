# DAY16: Drill — "Track the cookie jar"

Use curl to observe, store, and replay cookies.

## Part 1 — See cookies being set

```bash
# httpbin sets a cookie
curl -v https://httpbin.org/cookies/set?name=scott

# Look for Set-Cookie header in the response
# You should see: Set-Cookie: name=scott; Path=/
```

## Part 2 — Use a cookie jar

```bash
# Store cookies
curl -c /tmp/cookies.txt https://httpbin.org/cookies/set?name=scott

# Look at the jar
cat /tmp/cookies.txt

# Set another cookie
curl -c /tmp/cookies.txt https://httpbin.org/cookies/set?color=blue

# Check the jar again — now has two cookies
cat /tmp/cookies.txt

# Send stored cookies
curl -b /tmp/cookies.txt https://httpbin.org/cookies
# Expected: {"cookies": {"name": "scott", "color": "blue"}}
```

## Part 3 — Cookie expiry

```bash
# Set a cookie that expires quickly
curl -v "https://httpbin.org/cookies/set?temp=yes" 2>&1 | grep -i set-cookie

# Now check the cookies without sending
curl -b /tmp/cookies.txt https://httpbin.org/cookies
```

## Part 4 — Examine real cookies

```bash
# See what cookies a real site sets
curl -v https://github.com 2>&1 | grep -i set-cookie

# How many cookies does GitHub set?
# What flags do they use? (HttpOnly? Secure? SameSite?)
```

## Part 5 — Cookie security

```bash
# What happens if you visit an HTTP site that sets cookies?
# (They won't have the Secure flag)
curl -v http://httpbin.org/cookies/set?insecure=yes 2>&1 | grep -i set-cookie
```

## Part 6 — Build a cookie echo server in Go

Create `server.go`:

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    // Set a cookie on first visit
    http.SetCookie(w, &http.Cookie{
        Name:     "visit_count",
        Value:    "1",
        HttpOnly: true,
        Path:     "/",
    })

    // Read any existing cookies
    for _, c := range r.Cookies() {
        fmt.Fprintf(w, "Cookie: %s = %s\n", c.Name, c.Value)
    }
    if len(r.Cookies()) == 0 {
        fmt.Fprintf(w, "No cookies — first visit!")
    }
}

func main() {
    http.HandleFunc("/", handler)
    http.ListenAndServe(":8080", nil)
}
```

Run it, then:

```bash
curl -v http://localhost:8080
curl -v -b /tmp/cookies2.txt -c /tmp/cookies2.txt http://localhost:8080
```

## Deliverables

1. The content of your cookie jar after Part 2
2. Your Go echo server output
3. What flags did GitHub's cookies have?

## Hints

<details>
<summary>Click for hints</summary>

- `-c` writes cookies, `-b` reads them — you can use both together
- The cookie jar format has columns: domain, flag, path, secure, expiry, name, value
- To see cookies in the browser: DevTools → Application → Cookies
- `http.Cookie` struct has all the fields matching Set-Cookie attributes
</details>
