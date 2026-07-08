# DAY28: Drill — "Exploit and defend"

Simulate security vulnerabilities (in a safe, controlled environment) and then fix them.

## Part 1 — SQL injection simulation

Create `vulnerable.go`:

```go
package main

import (
    "database/sql"
    "fmt"
    "log"
    "net/http"
    _ "modernc.org/sqlite"
)

func main() {
    db, _ := sql.Open("sqlite", ":memory:")
    db.Exec("CREATE TABLE users (id INTEGER, name TEXT, email TEXT)")
    db.Exec("INSERT INTO users VALUES (1, 'admin', 'admin@example.com')")
    db.Exec("INSERT INTO users VALUES (2, 'alice', 'alice@example.com')")
    defer db.Close()

    http.HandleFunc("/user", func(w http.ResponseWriter, r *http.Request) {
        name := r.URL.Query().Get("name")

        // VULNERABLE: string concatenation
        query := fmt.Sprintf("SELECT id, name, email FROM users WHERE name = '%s'", name)
        row := db.QueryRow(query)

        var id int
        var uname, email string
        err := row.Scan(&id, &uname, &email)
        if err != nil {
            fmt.Fprintf(w, "Error: %s\nQuery: %s\n", err, query)
            return
        }
        fmt.Fprintf(w, "ID: %d\nName: %s\nEmail: %s\n", id, uname, email)
    })

    fmt.Println("Vulnerable server on :8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

Test the SQL injection:

```bash
# Normal query
curl "http://localhost:8080/user?name=admin"

# SQL injection — get all users
curl "http://localhost:8080/user?name=' OR 1=1 --"

# SQL injection — UNION
curl "http://localhost:8080/user?name=' UNION SELECT 1,'test','test@test.com' --"
```

Now fix it with a parameterized query:

```go
// SAFE
row := db.QueryRow("SELECT id, name, email FROM users WHERE name = ?", name)
```

## Part 2 — XSS simulation

Create a page that reflects user input:

```go
http.HandleFunc("/comment", func(w http.ResponseWriter, r *http.Request) {
    comment := r.URL.Query().Get("text")

    // VULNERABLE: directly outputting user input
    fmt.Fprintf(w, "<html><body><h1>Your comment:</h1><div>%s</div></body></html>", comment)
})
```

Test:

```bash
# Normal comment
curl "http://localhost:8080/comment?text=Hello!"

# XSS attack
curl "http://localhost:8080/comment?text=<script>alert('XSS')</script>"

# If you open this in a browser, the script executes!
```

Fix with proper escaping:

```go
import "html/template"
// Use html/template instead of fmt.Fprintf — it auto-escapes
tmpl := template.Must(template.New("page").Parse("<html><body><h1>Your comment:</h1><div>{{.}}</div></body></html>"))
tmpl.Execute(w, comment)
```

## Part 3 — CSRF simulation

Create a simple banking app that allows transfers via GET (vulnerable):

```go
// VULNERABLE: state change via GET
http.HandleFunc("/transfer", func(w http.ResponseWriter, r *http.Request) {
    to := r.URL.Query().Get("to")
    amount := r.URL.Query().Get("amount")
    fmt.Fprintf(w, "Transferred $%s to %s\n", amount, to)
    // In real app: db.Exec("UPDATE accounts SET balance = balance - ? WHERE id = ?")
})
```

```bash
# Attacker creates this page:
# <img src="http://localhost:8080/transfer?to=attacker&amount=1000" width="0" height="0">

# The browser automatically requests the image URL, including cookies!
```

Fix: require POST for state changes and include CSRF token.

## Part 4 — Secure the server

Add security headers to any Go server:

```go
func secureHeaders(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("X-Content-Type-Options", "nosniff")
        w.Header().Set("X-Frame-Options", "DENY")
        w.Header().Set("Content-Security-Policy", "default-src 'self'")
        next.ServeHTTP(w, r)
    })
}
```

## Deliverables

1. The SQL injection output (showing all users)
2. The XSS script reflected in the response
3. Your CSRF fix (changing GET to POST)
4. Security headers set in your server

## Hints

<details>
<summary>Click for hints</summary>

- Run the SQL injection server with `go run vulnerable.go` (install sqlite driver first: `go get modernc.org/sqlite`)
- The XSS example needs to be opened in a browser to actually execute
- CSRF requires the user to be logged in (cookies set) — for the exercise, just observe how GET-based state changes are dangerous
- `html/template` auto-escapes by default — `text/template` does NOT
- Always use `''//'` (or `$1`, `?`) for parameterized queries, never string concatenation
</details>
