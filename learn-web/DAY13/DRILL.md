# DAY13: Drill — "Inspect a JWT"

Decode and inspect JWTs, then send one in a request.

## Part 1 — Decode a JWT manually

Take this JWT:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IlNjb3R0Iiwicm9sZSI6ImFkbWluIiwiaWF0IjoxNTE2MjM5MDIyfQ.4xqBc8P5c1Yv0KJ0H0X8Z1w2K3M4N5O6P7Q8R9S0T1U
```

1. Split by `.` (dot)
2. Decode part 2 (the payload) with: `echo "BASE64_PART" | base64 -d`
3. What is the decoded payload?

## Part 2 — Verify with jwt.io

1. Go to `https://jwt.io`
2. Paste the token from Part 1
3. What algorithm is used?
4. What claims are in the payload?

## Part 3 — Use a JWT with an API

Some APIs accept JWTs:

```bash
# Generate a fake JWT for testing
PAYLOAD='{"sub":"test-user","name":"Scott","iat":1700000000,"exp":1999999999}'
HEADER='{"alg":"HS256","typ":"JWT"}'

# Encode parts (base64url safe)
B64HEADER=$(echo -n "$HEADER" | base64 -w0 | tr '+/' '-_' | tr -d '=')
B64PAYLOAD=$(echo -n "$PAYLOAD" | base64 -w0 | tr '+/' '-_' | tr -d '=')

# A real JWT needs a signature, but for testing, just append anything
JWT="$B64HEADER.$B64PAYLOAD.test-signature"
echo "Your JWT: $JWT"

# Try it with an API
curl -H "Authorization: Bearer $JWT" https://httpbin.org/bearer
```

## Part 4 — JWT expiry

```bash
# Create a JWT with an expired timestamp (iat in the past)
PAYLOAD_EXPIRED='{"sub":"test","exp":1000000000,"iat":1000000000}'
B64PAYLOAD=$(echo -n "$PAYLOAD_EXPIRED" | base64 -w0 | tr '+/' '-_' | tr -d '=')
JWT_EXPIRED="header.$B64PAYLOAD.signature"

# Try it
curl -H "Authorization: Bearer $JWT_EXPIRED" https://httpbin.org/bearer
```

## Part 5 — Go program to decode JWT

Create `main.go` that decodes a JWT without verifying:

```go
package main

import (
    "encoding/base64"
    "encoding/json"
    "fmt"
    "os"
    "strings"
)

func main() {
    if len(os.Args) < 2 {
        fmt.Println("Usage: go run main.go <jwt>")
        os.Exit(1)
    }
    parts := strings.Split(os.Args[1], ".")
    if len(parts) != 3 {
        fmt.Println("Invalid JWT")
        os.Exit(1)
    }
    // Decode payload (part index 1)
    // Hint: base64.RawURLEncoding.DecodeString()
    // Hint: json.Indent for pretty printing
    // Fill in the rest
}
```

## Deliverables

1. Decoded payload from Part 1
2. Your Go JWT decoder program
3. What status does httpbin return when you send a JWT?

## Hints

<details>
<summary>Click for hints</summary>

- JWT uses **base64url** encoding (not regular base64): `-` instead of `+`, `_` instead of `/`, no `=` padding
- Go's `base64.RawURLEncoding` decodes this correctly
- The `httpbin.org/bearer` endpoint checks for any valid `Authorization: Bearer` header
- An expired JWT will be accepted by httpbin (it doesn't verify signatures) but rejected by real servers
</details>
