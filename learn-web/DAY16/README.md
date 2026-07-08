# DAY16: Cookies — Set, Read, Expire, and Security Flags

---

## The anecdote: "Cookies are a loyalty card"

Your local coffee shop has a loyalty card:

| Coffee shop | Web cookies |
|-------------|-------------|
| You get a card on your first visit | Server sends `Set-Cookie` header on first request |
| The card says "Customer #42" | Cookie contains `session_id=abc123` |
| You show the card every visit | Browser sends `Cookie: session_id=abc123` every request |
| The card expires after a year | Cookie has an `Expires` or `Max-Age` |
| "Non-transferable" is printed on it | `HttpOnly` flag prevents JS from reading it |
| "Must use HTTPS" | `Secure` flag ensures cookie only sent over HTTPS |

---

## How cookies work

```
Browser                              Server
  |                                    |
  |--- GET / ------------------------>|
  |                                    |  "First visit, create session"
  |<--- 200 + Set-Cookie: id=abc123 --|  Server sends cookie
  |                                    |
  |--- GET /profile (Cookie: id=abc123) -->|
  |                                    |  "Ah, returning customer #abc123"
  |<--- 200 + data -------------------|
```

The browser **automatically** stores cookies and sends them with every request to the same domain.

---

## Setting a cookie (server-side)

The server sets a cookie with the `Set-Cookie` response header:

```text
Set-Cookie: session_id=abc123; Expires=Wed, 21 Oct 2025 07:28:00 GMT; HttpOnly; Secure; SameSite=Lax
```

| Attribute | What it does |
|-----------|--------------|
| `Expires` | When the cookie is deleted (HTTP date format) |
| `Max-Age` | Seconds until expiry (modern alternative to Expires) |
| `Domain` | Which domains can see this cookie |
| `Path` | Which URL path must the cookie be sent to |
| `HttpOnly` | JavaScript can't read this cookie (prevents XSS theft) |
| `Secure` | Only send this cookie over HTTPS |
| `SameSite` | Controls cross-site sending (Strict, Lax, None) |

---

## Viewing cookies with curl

```bash
# Create a cookie jar file and store cookies
curl -c /tmp/cookies.txt https://httpbin.org/cookies/set?name=scott

# Read the jar
cat /tmp/cookies.txt

# Send cookies from the jar
curl -b /tmp/cookies.txt https://httpbin.org/cookies
```

The `-c` flag writes cookies to a file. The `-b` flag reads cookies from a file.

---

## Cookie security flags — why they matter

| Attack | Cookie without protection | Cookie with protection |
|--------|--------------------------|------------------------|
| XSS steals session | `HttpOnly` missing → JS reads `document.cookie` | `HttpOnly` → JS can't access it |
| Man-in-the-middle | `Secure` missing → sent over HTTP | `Secure` → only sent over HTTPS |
| CSRF attack | `SameSite` missing → sent cross-site | `SameSite=Lax` → not sent on POST from other sites |

```bash
# Check cookies on a real site
curl -v https://github.com 2>&1 | grep -i set-cookie
```

---

## Cookies in Go

```go
// Server: Set a cookie
cookie := &http.Cookie{
    Name:     "session_id",
    Value:    "abc123",
    Path:     "/",
    HttpOnly: true,
    Secure:   true,
    SameSite: http.SameSiteLaxMode,
    MaxAge:   3600, // 1 hour
}
http.SetCookie(w, cookie)

// Server: Read a cookie
func handler(w http.ResponseWriter, r *http.Request) {
    cookie, err := r.Cookie("session_id")
    if err == http.ErrNoCookie {
        // No cookie — first visit
        return
    }
    // Use cookie.Value
}
```

---

## Senior mindset: "Cookies are the oldest auth mechanism"

Cookies were invented in 1994 by a Netscape engineer. They predate JWTs by over 20 years. Everything new in auth is just a remix of cookie concepts (expiry, secure flags, scoping).

**Rule of thumb:** If your frontend and backend are on the same domain, cookies are simpler and more secure than localStorage + Bearer tokens (because of HttpOnly + SameSite).

---

## Summary

| Concept | Cookie attribute | Effect |
|---------|-----------------|--------|
| Identity | `Set-Cookie: id=value` | Links request to a session |
| Persistence | `Expires` / `Max-Age` | Controls lifetime |
| Security | `HttpOnly` | Blocks JS access |
| Encryption | `Secure` | HTTPS only |
| CSRF protection | `SameSite` | Controls cross-site sending |
| curl store | `-c jar.txt` | Writes cookies to file |
| curl send | `-b jar.txt` | Reads cookies from file |
