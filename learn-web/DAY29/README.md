# DAY29: Final Project — Design a URL Shortener API

---

## The anecdote: "This is your capstone — everything connects here"

For 28 days you've learned:

- HTTP methods, status codes, headers, and bodies
- REST API design principles
- URL structure, query params, pagination
- Authentication and JWTs
- CORS, caching, rate limiting
- DNS, TLS, CDN, load balancing
- Security (XSS, CSRF, SQL injection)

Today you bring it all together. You won't write code — you'll **design** a URL shortener API. Design is harder than code. Good design makes coding trivial. Bad design makes coding impossible.

---

## The problem

Users want to turn long URLs into short ones:

```
Long URL:  https://example.com/blog/2026/07/web-fundamentals-complete-guide
Short URL: https://short.url/abc123
```

When someone visits `https://short.url/abc123`, they get redirected to the original URL.

---

## Requirements

### Functional

1. **Create a short URL** — given a long URL, return a short code
2. **Redirect** — given a short code, redirect to the original URL
3. **List your URLs** — show all URLs you've created (with pagination)
4. **Get URL stats** — how many times has a short URL been clicked?
5. **Delete a short URL** — remove it
6. **Update a short URL** — change the destination

### Non-functional

- Rate limited (100 requests/hour for unauthenticated users)
- Authenticated users get 1000 requests/hour
- Responses should be JSON
- Proper status codes everywhere
- CORS headers (so a web frontend can call the API)
- Cache-Control headers for the redirect (so browsers cache the redirect)

---

## Your design

For each endpoint, specify:

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/shorten` | POST | Create a short URL |
| `/api/{code}` | GET | Redirect to original URL |
| `/api/urls` | GET | List all URLs for current user |
| `/api/urls/{id}` | GET | Get stats for a specific URL |
| `/api/urls/{id}` | DELETE | Delete a short URL |
| `/api/urls/{id}` | PUT | Update the destination URL |

---

## Design each endpoint

### POST /api/shorten

**Request:**
```json
{
    "url": "https://example.com/very/long/url"
}
```

**Response (201 Created):**
```json
{
    "id": "url_abc123",
    "short_url": "https://short.url/abc123",
    "long_url": "https://example.com/very/long/url",
    "created_at": "2026-07-08T12:00:00Z"
}
```

**Error responses:**
- `400` — missing or invalid URL
- `422` — URL format invalid
- `429` — rate limited

### GET /api/{code}

**Response (302 Found):**
- `Location` header set to the original URL
- Body: empty

**Error responses:**
- `404` — code not found
- `410` — code was deleted

### GET /api/urls

**Response (200 OK):**
```json
{
    "data": [
        {
            "id": "url_abc123",
            "short_code": "abc123",
            "long_url": "https://example.com/long",
            "clicks": 42,
            "created_at": "2026-07-08T12:00:00Z"
        }
    ],
    "pagination": {
        "page": 1,
        "per_page": 10,
        "total": 1
    }
}
```

---

## Considerations

| Concern | Your decision |
|---------|--------------|
| How to generate short codes? | Random string? Hash? Counter? |
| How to handle duplicate URLs? | Create new? Return existing? |
| How to authenticate? | API key? JWT? Basic auth? |
| Where to store data? | In-memory? SQLite? Postgres? |
| How to handle redirect caching? | Cache-Control for 302? |
| How to handle CORS? | Allow all origins? Specific? |
| What if someone submits a malicious URL? | Validation? Scanning? |
| Should short codes be case-sensitive? | Pros and cons? |

---

## Senior mindset: "Design for the 90% case"

Your API should handle the common case elegantly:

1. **Creating a short URL** should be one POST
2. **Redirecting** should be one GET (fast, minimal overhead)
3. **Everything else** is secondary

Don't over-engineer. Don't plan for millions of users on day one. Design what works, build it well, and scale later.

---

## Deliverables

Create a file called `API_DESIGN.md` in DAY29 with:

1. A table of all endpoints (method, path, description)
2. Request/response schemas (JSON) for each endpoint
3. Status codes for each endpoint
4. Authentication design
5. Data model (what fields does a URL record have?)
6. Rate limiting strategy
7. CORS policy
8. Error response format

---

## Summary

| Aspect | What to decide |
|--------|---------------|
| Endpoints | Full RESTful API for URL management |
| Short code | How to generate unique codes |
| Auth | Simple (API key) or token-based |
| Storage | In-memory or persistent |
| Rate limiting | Token bucket or sliding window |
| CORS | Whitelist or open |
