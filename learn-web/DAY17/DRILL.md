# DAY17: Drill — "Run a session-based server"

Build and test a session-based login system in Go.

## Part 1 — Simple session server

Create `server.go`:

```go
package main

import (
    "crypto/rand"
    "encoding/hex"
    "fmt"
    "net/http"
    "sync"
    "time"
)

type Session struct {
    Username  string
    ExpiresAt time.Time
}

var (
    sessions = map[string]Session{}
    mu       sync.RWMutex
)

func generateSessionID() string {
    b := make([]byte, 16)
    rand.Read(b)
    return hex.EncodeToString(b)
}

func loginHandler(w http.ResponseWriter, r *http.Request) {
    if r.Method != "POST" {
        http.Error(w, "Use POST", http.StatusMethodNotAllowed)
        return
    }
    username := r.FormValue("username")
    password := r.FormValue("password")

    // In production, verify against a database
    if username != "admin" || password != "secret" {
        http.Error(w, "Bad credentials", http.StatusUnauthorized)
        return
    }

    sessionID := generateSessionID()
    mu.Lock()
    sessions[sessionID] = Session{
        Username:  username,
        ExpiresAt: time.Now().Add(1 * time.Hour),
    }
    mu.Unlock()

    http.SetCookie(w, &http.Cookie{
        Name:     "session_id",
        Value:    sessionID,
        HttpOnly: true,
        Path:     "/",
        MaxAge:   3600,
    })
    fmt.Fprintf(w, "Logged in as %s\n", username)
}

func profileHandler(w http.ResponseWriter, r *http.Request) {
    cookie, err := r.Cookie("session_id")
    if err != nil {
        http.Error(w, "Not logged in", http.StatusUnauthorized)
        return
    }
    mu.RLock()
    session, ok := sessions[cookie.Value]
    mu.RUnlock()
    if !ok || time.Now().After(session.ExpiresAt) {
        http.Error(w, "Session expired", http.StatusUnauthorized)
        return
    }
    fmt.Fprintf(w, "Welcome, %s!\n", session.Username)
}

func logoutHandler(w http.ResponseWriter, r *http.Request) {
    cookie, err := r.Cookie("session_id")
    if err == nil {
        mu.Lock()
        delete(sessions, cookie.Value)
        mu.Unlock()
    }
    http.SetCookie(w, &http.Cookie{
        Name:   "session_id",
        Value:  "",
        MaxAge: -1, // Delete cookie
    })
    fmt.Fprintf(w, "Logged out\n")
}

func main() {
    http.HandleFunc("/login", loginHandler)
    http.HandleFunc("/profile", profileHandler)
    http.HandleFunc("/logout", logoutHandler)
    http.ListenAndServe(":8080", nil)
}
```

## Part 2 — Test the session flow

```bash
# Start the server
go run server.go &

# Login (wrong password — expect 401)
curl -v -X POST -d "username=admin&password=wrong" http://localhost:8080/login

# Login (correct — get a session cookie)
curl -v -c /tmp/session.txt -X POST -d "username=admin&password=secret" \
  http://localhost:8080/login

# Access profile with the cookie
curl -b /tmp/session.txt http://localhost:8080/profile

# Access profile WITHOUT the cookie
curl http://localhost:8080/profile

# Logout
curl -b /tmp/session.txt -c /tmp/session.txt http://localhost:8080/logout

# Try profile again (should fail)
curl -b /tmp/session.txt http://localhost:8080/profile

# Kill the server
kill %1
```

## Part 3 — Challenge: add expiry check

Modify the profile handler to return a specific error message when the session is expired.

## Deliverables

1. Output from each step in Part 2
2. What happens when you access `/profile` without logging in?
3. What happens after logout?

## Hints

<details>
<summary>Click for hints</summary>

- The cookie jar (`-c`) stores the session ID returned by the server
- Without the cookie, the server sees no session — returns 401
- After logout, the session is deleted from the map AND the cookie is cleared (MaxAge=-1)
- `sync.RWMutex` protects concurrent map access — sessions can be accessed by multiple goroutines
</details>
