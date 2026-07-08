# DAY14: CORS — Cross-Origin Resource Sharing

---

## The anecdote: "CORS is a club guest list"

You run a nightclub. Your bouncer has a list:

| Guest | On the list? | Allowed in? |
|-------|-------------|-------------|
| Alice | ✅ Yes | Come in! |
| Bob | ✅ Yes | Come in! |
| Mallory | ❌ No (blacklisted) | Get lost! |
| Eve | ❌ No (unknown) | Sorry, your name's not on the list |

The club is your API. The bouncer is the browser's CORS policy. **Who can access your API from their website?** That's what CORS controls.

---

## The Same-Origin Policy

By default, a web page from `https://site-a.com` **cannot** make requests to `https://api-site-b.com`. This is the **Same-Origin Policy** — the browser's built-in security rule.

Two URLs are "same origin" if they share:

```
Same:  https://example.com/page1  and  https://example.com/page2
       \___/ \_________/
      scheme    host (port is implied)
        
Different:  https://example.com  and  https://api.example.com
            (host is different)
            
Different:  https://example.com  and  http://example.com
            (scheme is different)
```

---

## Why CORS exists

Without CORS, any malicious website could:

1. Load `https://your-bank.com/transfer?to=hacker&amount=10000`
2. Read the response (if your bank uses cookies for auth)
3. Steal your money

The browser prevents this. Your bank's server must explicitly say "this other website is allowed to access me."

---

## How CORS works — the headers

The server must include specific headers in its response:

```text
Access-Control-Allow-Origin: https://my-app.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 3600
```

---

## Simple requests vs Preflighted requests

**Simple request** (GET, HEAD, POST with simple content types) — browser just checks the response headers:

```
Browser → Server (GET /data)
Server → Browser (200 + Access-Control-Allow-Origin: *)
Browser: "The server allows my origin, I can read this response ✅"
```

**Preflighted request** (PUT, DELETE, custom headers, JSON) — browser sends a preflight OPTIONS request first:

```
Browser → Server (OPTIONS /data)  ← Preflight check
          Headers: Origin, Access-Control-Request-Method
Server → Browser (204 + Allow-Origin + Allow-Methods)
Browser: "The server says I'm allowed, now send the real request"

Browser → Server (DELETE /data)   ← Actual request
Server → Browser (200 + CORS headers)
```

---

## See CORS in action

```bash
# Server's perspective — no CORS issue
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
# Works fine! (curl doesn't enforce CORS)

# Browser responds differently
# Open DevTools Console and run:
# fetch('https://jsonplaceholder.typicode.com/posts/1', {method: 'DELETE'})
# You'll see a CORS error (if the server doesn't allow it)
```

**CORS is enforced by the browser, not the server.** curl, Postman, server-to-server calls — none of them enforce CORS.

---

## CORS in a Go server

```go
func corsMiddleware(next http.Handler) http.Handler {
    return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        w.Header().Set("Access-Control-Allow-Origin", "https://my-app.com")
        w.Header().Set("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE, OPTIONS")
        w.Header().Set("Access-Control-Allow-Headers", "Content-Type, Authorization")

        // Handle preflight
        if r.Method == "OPTIONS" {
            w.WriteHeader(http.StatusNoContent)
            return
        }

        next.ServeHTTP(w, r)
    })
}
```

---

## Common CORS errors

| Error message | Cause |
|--------------|-------|
| `No 'Access-Control-Allow-Origin' header is present` | Server didn't include the header |
| `Response to preflight request doesn't pass access control check` | Preflight (OPTIONS) response is wrong |
| `Credentials flag is 'true', but the 'Access-Control-Allow-Origin' is '*'` | Can't use credentials with wildcard origin |
| `Method PUT is not allowed by Access-Control-Allow-Methods` | Server didn't list the method |

---

## Senior mindset: "CORS is a browser-only concept"

CORS does **not** protect your API. Any server-side tool (curl, Postman, a backend service) can bypass CORS.

Your API needs real authentication (JWT, API keys, session cookies). CORS only tells the browser which origins are allowed to make requests from JavaScript.

---

## Summary

| Concept | Description |
|---------|-------------|
| Same-Origin Policy | Browser blocks cross-origin requests by default |
| CORS | Server tells the browser which origins are allowed |
| Simple request | GET/POST with basic content types — no preflight |
| Preflight | OPTIONS request before PUT/DELETE/custom headers |
| Server headers | `Access-Control-Allow-Origin`, `-Methods`, `-Headers` |
| Enforced by | Browser only (curl, Postman, backend → no CORS) |
