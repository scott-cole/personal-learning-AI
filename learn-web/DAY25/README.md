# DAY25: Reverse Proxies — The Receptionist

---

## The anecdote: "A reverse proxy is a receptionist"

You walk into a large office building:

| Without receptionist | With receptionist |
|---------------------|------------------|
| You have to find the right office yourself | Receptionist asks who you're here to see |
| You wander around lost | Receptionist directs you to the right person |
| Anyone can walk into any office | Receptionist screens visitors |
| If the person is busy, you wait in the hallway | Receptionist tells you to wait in the lobby |
| If the building has multiple floors, you need to know which one | Receptionist knows everyone's location |

A **reverse proxy** is the receptionist for your servers. It sits in front of one or more backend servers and handles incoming requests.

---

## What a reverse proxy does

```
Client                          Reverse Proxy                    Backend Server
  |                                |                                |
  |--- https://example.com ------>|                                |
  |                                |--- http://localhost:3000 ---->|
  |                                |<--- response -----------------|
  |<--- response -----------------|                                |
```

| Task | How the proxy helps |
|------|-------------------|
| TLS termination | Handles HTTPS so backends don't have to |
| Load balancing | Distributes requests across servers |
| Caching | Serves cached responses without hitting backends |
| Compression | Compresses responses before sending to clients |
| Rate limiting | Blocks abusive traffic before it reaches backends |
| Routing | Sends `/api/*` to one server, `/app/*` to another |
| Security | Hides backend IPs, filters malicious requests |

---

## nginx as a reverse proxy

nginx is the most popular reverse proxy. Here's a minimal config:

```nginx
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Now clients connect to nginx on port 80, and nginx forwards to your Go app on port 3000.

---

## Why not expose your app directly?

| Direct exposure | Behind reverse proxy |
|----------------|---------------------|
| `http://localhost:3000` | `https://example.com` |
| Need to handle TLS manually | Proxy handles TLS (Let's Encrypt) |
| Need rate limiting in app code | Proxy handles rate limiting |
| No caching | Proxy can cache static assets |
| Can't scale horizontally | Proxy load balances across instances |
| Exposed to internet directly | Backend is isolated |

---

## Headers added by reverse proxies

```bash
# See what headers a proxy adds
curl -H "Host: example.com" https://httpbin.org/headers
```

Standard headers that proxies add:

| Header | What it contains |
|--------|-----------------|
| `X-Forwarded-For` | Original client IP |
| `X-Forwarded-Proto` | Original protocol (http or https) |
| `X-Forwarded-Host` | Original Host header |
| `X-Real-IP` | Client IP (nginx) |

Your Go app needs to use these headers to know the real client IP:

```go
func getClientIP(r *http.Request) string {
    if fwd := r.Header.Get("X-Forwarded-For"); fwd != "" {
        return strings.Split(fwd, ",")[0]
    }
    return r.RemoteAddr // Direct connection (no proxy)
}
```

---

## Routing different paths to different backends

```nginx
# nginx routing
server {
    listen 80;
    server_name api.example.com;

    location /api/v1/ {
        proxy_pass http://localhost:3000;  # Go API
    }

    location /api/v2/ {
        proxy_pass http://localhost:3001;  # New Go API
    }

    location /static/ {
        alias /var/www/static;  # Serve files directly
        expires 365d;
    }
}
```

---

## Senior mindset: "Start simple, add proxies when you need them"

A reverse proxy adds complexity. Start with your app directly exposed, then add nginx when you need:

- Multiple backend instances (load balancing)
- TLS termination (Let's Encrypt)
- Rate limiting
- Path-based routing to different services

**For personal projects:** You might never need one (a single Go binary is fine).
**For production:** You almost always use one (nginx, Caddy, Cloudflare).

---

## Summary

| Feature | Direct app | Reverse proxy |
|---------|-----------|---------------|
| TLS | Manual | Automatic (Let's Encrypt) |
| Rate limiting | In app code | At proxy level |
| Load balancing | Single instance | Multiple instances |
| Static files | Handled by app | Served directly by nginx |
| Client IP | `r.RemoteAddr` | `X-Forwarded-For` header |
| Complexity | Simple | More moving parts |
