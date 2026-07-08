# DAY03: HTTP Status Codes — 1xx to 5xx

---

## The anecdote: "Status codes are traffic lights"

You're driving. Up ahead you see:

| Colour | Meaning | HTTP equivalent |
|--------|---------|-----------------|
| 🟢 **Green** | Go ahead, everything's fine | **2xx Success** |
| 🟡 **Yellow** | Caution, something's happening | **1xx / 3xx Informational / Redirect** |
| 🔴 **Red** | Stop, something went wrong | **4xx / 5xx Error** |

Just like traffic lights, status codes give you immediate feedback: *Keep going*, *do something else*, or *stop and fix something*.

---

## 1xx — Informational (rarely seen)

"Hold on, I'm processing your request."

| Code | Name | Meaning |
|------|------|---------|
| 100 | Continue | "Keep sending the body" |
| 101 | Switching Protocols | "Switching to WebSocket" |
| 102 | Processing | "Still working on it" (WebDAV) |

You almost never encounter 1xx codes in normal browsing. They're handled automatically by your HTTP client.

---

## 2xx — Success

"Everything worked."

| Code | Name | Meaning |
|------|------|---------|
| 200 | OK | "Here's your data" (most common) |
| 201 | Created | "Resource created" (POST) |
| 202 | Accepted | "I'll process this later" (async) |
| 204 | No Content | "Success, but nothing to show" (DELETE) |

```bash
curl -v https://example.com
# Expect: 200 OK

curl -v -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"test","body":"test","userId":1}'
# Expect: 201 Created
```

---

## 3xx — Redirection

"The resource is somewhere else. Go here instead."

| Code | Name | Meaning |
|------|------|---------|
| 301 | Moved Permanently | "This URL is dead, use the new one" |
| 302 | Found | "Temporary redirect" |
| 304 | Not Modified | "Use your cached version" |
| 307 | Temporary Redirect | "Like 302, but keep the method" |
| 308 | Permanent Redirect | "Like 301, but keep the method" |

```bash
curl -v http://example.com
# Note: without the 's', this redirects to https://example.com (301)
```

Most browsers follow 3xx redirects automatically. curl follows them by default (max 50 redirects). Use `-L` to follow redirects explicitly, or `--no-location` to stop.

---

## 4xx — Client Error

"You made a mistake. Fix your request."

| Code | Name | Meaning |
|------|------|---------|
| 400 | Bad Request | "Malformed syntax" |
| 401 | Unauthorized | "You need to log in" |
| 403 | Forbidden | "Logged in but not allowed" |
| 404 | Not Found | "Resource doesn't exist" |
| 405 | Method Not Allowed | "GET this, not DELETE" |
| 408 | Request Timeout | "You took too long" |
| 409 | Conflict | "Version mismatch / duplicate" |
| 422 | Unprocessable Entity | "Validation failed" |
| 429 | Too Many Requests | "Slow down!" |

```bash
curl -v https://example.com/nonexistent
# Expect: 404 Not Found
```

---

## 5xx — Server Error

"Something broke on my end. It's not you, it's me."

| Code | Name | Meaning |
|------|------|---------|
| 500 | Internal Server Error | "Something crashed" |
| 502 | Bad Gateway | "Upstream server failed" |
| 503 | Service Unavailable | "Down for maintenance / overloaded" |
| 504 | Gateway Timeout | "Upstream server didn't respond in time" |

```bash
curl -v https://httpbin.org/status/500
# Expect: 500 Internal Server Error
```

---

## The cheat sheet

```
1xx — Hold on…
2xx — Here you go!
3xx — Go away (over there)
4xx — You messed up
5xx — I messed up
```

---

## Senior mindset: "Status codes are contract terms"

When you build an API, choosing the *correct* status code is part of your contract with the client.

A common junior mistake: returning `200 OK` for everything, even errors. Don't do that. If the client sends bad data, return `400 Bad Request`. If they're not authenticated, return `401 Unauthorized`. The status code is the first thing a client checks — make it meaningful.

---

## Summary

| Range | Category | Meaning |
|-------|----------|---------|
| 1xx | Informational | Processing |
| 2xx | Success | Everything worked |
| 3xx | Redirection | Go somewhere else |
| 4xx | Client Error | You made a mistake |
| 5xx | Server Error | Server broke something |
