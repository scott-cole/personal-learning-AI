# DAY13: JWT Tokens — Structure, Lifecycle, and Usage

---

## The anecdote: "A JWT is a stamped passport"

Your passport has three parts:

1. **Header** — "This is a passport, version 1" (metadata)
2. **Payload** — Name, photo, nationality (your identity)
3. **Signature** — Official stamp that proves it's real (verified by the issuing government)

A JWT (JSON Web Token) works the same way:

```
Header + Payload + Signature
```

The server creates it, signs it with a secret, and gives it to the client. The client presents it on every request. The server verifies the signature to ensure it wasn't tampered with.

---

## JWT structure

A JWT looks like this:

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJhZG1pbiJ9.4fZ8MxB3QJx9Yq3Lq5X7z9w0K2c4d6f8
    \___________________________/   \___________________________/   \___________________________/
            Header (alg + type)              Payload (claims)                  Signature
```

Each part is base64url-encoded JSON, separated by dots.

**Header:**

```json
{
    "alg": "HS256",
    "typ": "JWT"
}
```

**Payload (claims):**

```json
{
    "sub": "user_123",
    "name": "Scott",
    "role": "admin",
    "iat": 1700000000,
    "exp": 1700086400
}
```

**Signature:** Created by hashing `header.payload` with a secret key.

---

## Decode a JWT (without verifying)

```bash
# Paste your JWT and decode it
echo "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoxMjMsInJvbGUiOiJhZG1pbiJ9.4fZ8MxB3QJx9Yq3Lq5X7z9w0K2c4d6f8" | cut -d. -f2 | base64 -d 2>/dev/null || echo "decoded"
```

Or use [`jwt.io`](https://jwt.io) to decode and verify.

**Security warning:** Anyone can decode the payload (it's just base64). Don't put secrets in a JWT. The signature only prevents **tampering**, not **reading**.

---

## Common JWT claims

| Claim | Full name | What it means |
|-------|-----------|---------------|
| `sub` | Subject | Who this token is for (usually user ID) |
| `iat` | Issued At | When the token was created (Unix timestamp) |
| `exp` | Expiration | When the token expires (Unix timestamp) |
| `nbf` | Not Before | Token isn't valid before this time |
| `iss` | Issuer | Who created the token |
| `aud` | Audience | Who the token is intended for |

---

## JWT in Go (sending, not creating)

```go
import (
    "fmt"
    "net/http"
    "time"
)

func callAPI() {
    req, _ := http.NewRequest("GET", "https://api.example.com/protected", nil)
    
    // The JWT you got from logging in
    token := "eyJhbGciOiJIUzI1NiIs..."
    req.Header.Set("Authorization", "Bearer " + token)

    client := &http.Client{Timeout: 10 * time.Second}
    resp, _ := client.Do(req)
    defer resp.Body.Close()

    fmt.Println("Status:", resp.Status)
}
```

---

## JWT lifecycle

```
Client                       Server
  |                            |
  |--- POST /login (creds) --->|
  |                            |  Verify password
  |<--- 200 + JWT ------------|  Create & sign JWT
  |                            |
  |--- GET /data (Bearer) ---->|  Verify JWT signature
  |                            |  Check expiry
  |<--- 200 + data -----------|  Extract user info from claims
  |                            |
```

---

## Why JWT?

| Feature | Benefit |
|---------|---------|
| Stateless | Server doesn't need a session store — everything is in the token |
| Portable | Works across services (microservices can verify the same token) |
| Self-contained | User ID, role, permissions all in the token |
| Standard | RFC 7519, wide language support |

**The tradeoff:** JWTs can't be revoked easily (until they expire). If someone steals your JWT, they can use it until it expires.

---

## Senior mindset: "JWTs are for verification, not encryption"

Your JWT payload is **base64 encoded**, not **encrypted**. Anyone who has the token can read the claims.

- ✅ Put in JWT: user ID, role, expiry
- ❌ Put in JWT: password, credit card, secrets

If you need the payload to be hidden, use **JWE** (JSON Web Encryption) — but that's rare.

---

## Summary

| Component | Description | Example |
|-----------|-------------|---------|
| Header | Algorithm and type | `{"alg":"HS256"}` |
| Payload | Claims (data) | `{"sub":"user_123","exp":1700000000}` |
| Signature | Prevents tampering | HMAC of header.payload |
| Usage | `Authorization: Bearer <token>` | Sent on every authenticated request |
