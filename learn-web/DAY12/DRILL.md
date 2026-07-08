# DAY12: Drill — "Authenticate everything"

Practice sending authenticated requests using curl and Go.

## Part 1 — Basic Auth with httpbin

httpbin.org has a test endpoint:

```bash
# Correct credentials
curl -v -u admin:secret https://httpbin.org/basic-auth/admin/secret
# Expected: 200 OK

# Wrong password
curl -v -u admin:wrongpass https://httpbin.org/basic-auth/admin/secret
# Expected: 401 Unauthorized

# No auth
curl -v https://httpbin.org/basic-auth/admin/secret
# Expected: 401 Unauthorized
```

## Part 2 — Bearer token with GitHub API

GitHub allows unauthenticated requests (rate limited to 60/hour). Add auth for 5000/hour:

```bash
# Without auth (rate limited)
curl -I https://api.github.com/users/scott

# With Personal Access Token (rate: 5000/hour)
curl -H "Authorization: Bearer YOUR_TOKEN" https://api.github.com/users/scott
```

Don't have a token? Create one at `github.com/settings/tokens`.

## Part 3 — Send auth in Go

Create `main.go` that:
1. Takes a URL and an API key as arguments
2. Sends a GET request with `Authorization: Bearer <key>`
3. Prints the status code and first 200 characters of the response

```go
package main

import (
    "fmt"
    "io"
    "net/http"
    "os"
    "time"
)

func main() {
    if len(os.Args) < 3 {
        fmt.Println("Usage: go run main.go <url> <token>")
        os.Exit(1)
    }
    url := os.Args[1]
    token := os.Args[2]

    // TODO: create request, add auth header, send, print response
}
```

## Part 4 — Expose your own credentials

What if you accidentally commit an API key to a public repo?

```bash
# Search GitHub for exposed keys (educational purposes only)
# Attackers scrape repos for patterns like "api_key", "password", "Bearer"
```

## Deliverables

1. Output from Part 1 (both 200 and 401)
2. Your Go program from Part 3
3. What happens when you send a request with an invalid bearer token?

## Hints

<details>
<summary>Click for hints</summary>

- GitHub tokens: create at Settings → Developer settings → Personal access tokens
- `req.Header.Set("Authorization", "Bearer "+token)`
- Without auth, GitHub returns `401` for private repos and `200` for public (but rate-limited)
- Always use environment variables or secret managers for tokens, never hardcode them
</details>
