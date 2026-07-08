# DAY04: HTTP Headers — Request & Response

---

## The anecdote: "Headers are the envelope on a letter"

When you mail a letter, the envelope has information the postal service needs:

- **To address** — where it's going
- **From address** — who sent it
- **Stamp** — postage paid
- **Fragile sticker** — handle with care
- **Date mark** — when it was posted

The letter inside is the **body**. The envelope is the **headers**.

HTTP headers work the same way — they're metadata about the request or response that the server or client uses to handle the message correctly.

---

## Request headers (sent by the client)

```bash
curl -v https://api.github.com/users/scott
```

You'll see headers like:

```
> GET /users/scott HTTP/1.1
> Host: api.github.com
> User-Agent: curl/8.0
> Accept: */*
```

| Header | Purpose | Example |
|--------|---------|---------|
| `Host` | Which server (required for virtual hosting) | `api.github.com` |
| `User-Agent` | Who is the client? | `curl/8.0`, `Mozilla/5.0` |
| `Accept` | What content types do you understand? | `application/json` |
| `Accept-Language` | What language do you prefer? | `en-US` |
| `Authorization` | Who are you? (credentials) | `Bearer <token>` |
| `Content-Type` | What format is the body? | `application/json` |
| `Content-Length` | How big is the body in bytes? | `42` |

---

## Response headers (sent by the server)

The server sends back its own envelope:

```
< HTTP/1.1 200 OK
< Content-Type: application/json; charset=utf-8
< Content-Length: 1234
< Cache-Control: max-age=60
< Set-Cookie: session_id=abc123
< X-Request-Id: 7a8b9c
```

| Header | Purpose | Example |
|--------|---------|---------|
| `Content-Type` | What format is this response? | `text/html`, `application/json` |
| `Content-Length` | How many bytes? | `4567` |
| `Cache-Control` | Can the client cache this? | `max-age=3600`, `no-cache` |
| `Set-Cookie` | Store this cookie | `session=abc123; HttpOnly` |
| `X-Request-Id` | Trace ID for debugging | `req-12345` |
| `Location` | Redirect to this URL | `https://example.com/new` |

---

## Content-Type is the most important header

`Content-Type` tells the client how to interpret the body. The value is a **MIME type**:

| MIME type | Meaning |
|-----------|---------|
| `text/html` | HTML page |
| `application/json` | JSON data |
| `application/xml` | XML data |
| `text/plain` | Plain text |
| `image/png` | PNG image |
| `multipart/form-data` | File upload |

**Without Content-Type, the client doesn't know if the body is JSON, HTML, or a potato recipe.**

```bash
# Ask for JSON specifically
curl -H "Accept: application/json" https://api.github.com/users/scott

# Send JSON
curl -X POST https://example.com/api \
  -H "Content-Type: application/json" \
  -d '{"key":"value"}'
```

---

## Custom headers (X- headers)

Traditionally, custom headers started with `X-`. Modern convention drops the `X-` prefix.

```bash
# Send a custom trace header
curl -H "X-Trace-Id: my-trace-123" https://example.com
```

---

## Headers are case-insensitive

`Content-Type`, `content-type`, and `CONTENT-TYPE` are the same header. By convention, we use Title-Case.

---

## Senior mindset: "Headers are your debugging breadcrumbs"

When debugging a production issue, headers tell the story:

- `X-Request-Id` — trace a single request across multiple services
- `Cache-Control` — figure out why a stale response was served
- `X-RateLimit-Remaining` — see if you hit rate limits

Always add a unique request ID header to your applications. Future you will thank present you when you're hunting a bug at 2 AM.

---

## Summary

| Header type | Examples | Purpose |
|-------------|----------|---------|
| Request | `Host`, `User-Agent`, `Accept`, `Authorization` | Tell the server about the client |
| Response | `Content-Type`, `Cache-Control`, `Set-Cookie` | Tell the client about the response |
| Content | `Content-Type`, `Content-Length`, `Content-Encoding` | Describe the body |
| Custom | `X-Request-Id`, `X-Trace-Id` | Debugging / tracing |
