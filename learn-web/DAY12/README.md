# DAY12: Authentication — Basic Auth and API Keys

---

## The anecdote: "Auth is a gym membership card"

You walk into a gym:

1. **At the front desk** you show your membership card
2. The staff checks: is the card valid? Is it yours? Is it expired?
3. If everything checks out: "Welcome, go work out"
4. If not: "Sorry, you can't enter"

HTTP authentication works the same way. Every request carries credentials. The server checks them and decides: let you in (200) or turn you away (401).

---

## Why authentication?

Without auth, anyone can access any resource:

```bash
# Anyone can read your data
curl https://api.example.com/users/my-private-data

# Anyone can delete your account
curl -X DELETE https://api.example.com/users/42
```

Auth ensures only the right people can access protected resources.

---

## Basic Auth — The simplest form

HTTP Basic Auth sends a username and password encoded in base64:

```bash
# Syntax: -u username:password
curl -u admin:secret https://httpbin.org/basic-auth/admin/secret
```

What curl actually sends:

```
GET /basic-auth/admin/secret HTTP/1.1
Host: httpbin.org
Authorization: Basic YWRtaW46c2VjcmV0
```

`YWRtaW46c2VjcmV0` is `admin:secret` base64-encoded.

**Security warning:** Basic Auth without HTTPS is plaintext. Anyone on the network can read your password. **Never use Basic Auth without HTTPS.**

```bash
# How to encode/decode base64 (for curiosity)
echo -n "admin:secret" | base64
# Output: YWRtaW46c2VjcmV0

echo "YWRtaW46c2VjcmV0" | base64 -d
# Output: admin:secret
```

---

## API Keys — The most common pattern

An API key is a unique identifier (usually a long random string) that identifies the client:

```bash
# As a header (most common)
curl -H "X-API-Key: abc123def456" https://api.example.com/data

# As a query parameter
curl "https://api.example.com/data?api_key=abc123def456"

# As a bearer token
curl -H "Authorization: Bearer abc123def456" https://api.example.com/data
```

**API Key best practices:**
- Generate keys with high entropy (`openssl rand -hex 32`)
- Never commit keys to git
- Rotate keys periodically
- Scope keys to specific permissions
- Allow multiple keys per user (so one can be revoked without breaking everything)

---

## In Go: Sending auth

```go
package main

import (
    "fmt"
    "net/http"
    "time"
)

func main() {
    client := &http.Client{Timeout: 10 * time.Second}

    // Method 1: Basic Auth
    req, _ := http.NewRequest("GET", "https://httpbin.org/basic-auth/admin/secret", nil)
    req.SetBasicAuth("admin", "secret")

    // Method 2: API Key header
    req.Header.Set("X-API-Key", "abc123def456")

    // Method 3: Bearer token
    req.Header.Set("Authorization", "Bearer abc123def456")

    resp, _ := client.Do(req)
    defer resp.Body.Close()

    fmt.Println("Status:", resp.Status)
}
```

---

## Server-side: Checking credentials

```go
func authMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        // Check API key header
        key := r.Header.Get("X-API-Key")
        if key != "expected-key" {
            http.Error(w, "Unauthorized", http.StatusUnauthorized)
            return
        }
        next.ServeHTTP(w, r)
    })
}
```

---

## Senior mindset: "Authentication is not authorisation"

| Auth type | What it answers | Example |
|-----------|----------------|---------|
| **Authentication** | *Who are you?* | `admin:secret` — logged in |
| **Authorisation**  | *What can you do?* | Admin can DELETE, user can only GET |

Authentication proves identity. Authorisation proves permission. They're two separate concerns. Always check both.

---

## Summary

| Method | curl flag | Header sent |
|--------|-----------|-------------|
| Basic Auth | `-u user:pass` | `Authorization: Basic <base64>` |
| API Key (header) | `-H "X-API-Key: key"` | `X-API-Key: <key>` |
| API Key (query) | `?api_key=key` | In URL |
| Bearer token | `-H "Authorization: Bearer token"` | `Authorization: Bearer <token>` |
