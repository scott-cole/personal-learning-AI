# DAY17: Sessions — Server-Side vs Client-Side

---

## The anecdote: "Sessions are a hotel key card"

You check into a hotel:

| Hotel | Web sessions |
|-------|-------------|
| You give your ID at check-in | User logs in with credentials |
| Front desk creates your room key | Server creates a **session** (stored in DB/memory) |
| The key has room number encoded | Session ID stored in a cookie |
| You show the key to enter your room | Browser sends cookie, server looks up session |
| At checkout, key is deactivated | Session is deleted — no more access |
| If you lose the key, front desk can issue a new one | Session is still on the server — just need a new cookie |

---

## What is a session?

A **session** is server-side storage linked to a client via a session ID (stored in a cookie).

```
Client                            Server
  |                                 |
  |--- POST /login (user+pass) --->|
  |                                 |  Verify credentials
  |                                 |  Create session (store in DB)
  |<--- Set-Cookie: sid=abc123 ----|  Return session ID in cookie
  |                                 |
  |--- GET /profile (Cookie: sid=abc123) -->|
  |                                 |  Look up session by sid
  |                                 |  "Ah, user #42"
  |<--- 200 + profile data --------|
```

---

## Server-side sessions (traditional)

```go
// Simplified session store
var sessions = map[string]Session{}

type Session struct {
    UserID    int
    CreatedAt time.Time
    ExpiresAt time.Time
}

func loginHandler(w http.ResponseWriter, r *http.Request) {
    // Verify credentials...
    userID := 42

    // Create session
    sessionID := generateRandomString(32)
    sessions[sessionID] = Session{
        UserID:    userID,
        CreatedAt: time.Now(),
        ExpiresAt: time.Now().Add(24 * time.Hour),
    }

    // Set cookie
    http.SetCookie(w, &http.Cookie{
        Name:     "session_id",
        Value:    sessionID,
        HttpOnly: true,
        Secure:   true,
        Path:     "/",
        Expires:  time.Now().Add(24 * time.Hour),
    })
}

func authMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        cookie, err := r.Cookie("session_id")
        if err != nil {
            http.Error(w, "Unauthorized", http.StatusUnauthorized)
            return
        }
        session, ok := sessions[cookie.Value]
        if !ok || time.Now().After(session.ExpiresAt) {
            http.Error(w, "Session expired", http.StatusUnauthorized)
            return
        }
        // Add user info to context
        next.ServeHTTP(w, r)
    })
}
```

---

## Client-side sessions (JWT-style)

Instead of storing data on the server, you store it in the cookie itself (encrypted):

```go
func createSessionToken(userID int) (string, error) {
    claims := jwt.MapClaims{
        "user_id": userID,
        "exp":     time.Now().Add(24 * time.Hour).Unix(),
    }
    token := jwt.NewWithClaims(jwt.SigningMethodHS256, claims)
    return token.SignedString([]byte("secret-key"))
}
```

**Tradeoff:** No server storage needed, but can't revoke individual sessions.

---

## Session stores (where data lives)

| Store | Speed | Persistence | Best for |
|-------|-------|-------------|----------|
| In-memory (map) | Fastest | Lost on restart | Dev, single-server |
| Redis | Fast | Yes | Production, multi-server |
| Database (Postgres) | Slower | Yes | When sessions need querying |
| Cookie (encrypted) | No server store | Lost on cookie clear | Stateless apps |

---

## Session security checklist

- ✅ Regenerate session ID on login (prevent session fixation)
- ✅ Set `HttpOnly`, `Secure`, `SameSite` on session cookies
- ✅ Expire sessions (don't keep them forever)
- ✅ Store minimum data in session (user ID, not the whole user object)
- ❌ Never store passwords in sessions
- ✅ Rotate secret keys periodically

---

## Senior mindset: "Sessions vs JWTs is a false dichotomy"

They solve different problems:

| Problem | Solution |
|---------|----------|
| I need to know who is making requests | Both sessions and JWTs work |
| I need to revoke access immediately | Server-side sessions (delete from store) |
| I'm building a microservice architecture | JWTs (no shared session store needed) |
| I have a monolith with server-side rendering | Sessions (simpler, more secure) |
| I have a mobile app + web frontend | JWTs (works across platforms) |

---

## Summary

| Feature | Server-side sessions | Client-side (JWT) |
|---------|---------------------|-------------------|
| Where data lives | Server (DB/Redis/memory) | In the token itself |
| Storage needed | Yes | No |
| Revocable | Yes (delete from store) | No (until expiry) |
| Cookie needed | Yes (session ID) | Optional |
| Cross-platform | Tricky (cookie-based) | Easy (Bearer header) |
