# DAY18: Caching — Cache-Control, ETags, and Browser Caching

---

## The anecdote: "Caching is a fridge — store leftovers for later"

You cook dinner:

| Without a fridge | With a fridge |
|-----------------|---------------|
| Cook every meal from scratch | Cook once, store leftovers |
| Go to the store every time you're hungry | Grab from the fridge |
| Takes 45 minutes each time | Takes 2 minutes |
| Wastes food (can't keep it) | Throws away after 3 days (expiry) |

HTTP caching is exactly this: instead of fetching a resource from the server every time, the browser (or a proxy) stores it and reuses it.

---

## Why caching matters

| Metric | Without cache | With cache |
|--------|--------------|------------|
| Page load time | 3 seconds | 300ms |
| Server requests per page | 100 | 5 |
| Bandwidth | 2MB | 100KB |
| Server CPU | 100% | 5% |

---

## Cache-Control header

The `Cache-Control` response header tells the browser and proxies how to cache:

```text
Cache-Control: public, max-age=3600
```

| Directive | Meaning |
|-----------|---------|
| `public` | Anyone can cache (browser + proxies) |
| `private` | Only the browser can cache (not shared proxies) |
| `no-cache` | Must revalidate with the server before using cache |
| `no-store` | Don't cache at all (sensitive data) |
| `max-age=3600` | Cache for 3600 seconds (1 hour) |
| `must-revalidate` | Once expired, must check server before reuse |

---

## ETags — "Has this changed?"

An **ETag** (Entity Tag) is a hash of the resource content:

```
First request:
Client → Server
Server → Client: 200 + Content + ETag: "abc123"

Second request (conditional):
Client → Server: If-None-Match: "abc123"
Server → Client: 304 Not Modified (no body)
                  → Browser uses cached copy
```

```bash
# First request — get the ETag
curl -I https://api.github.com/users/scott
# Look for: etag: "abc123def..."

# Second request — send the ETag back
curl -H "If-None-Match: \"abc123def...\"" -v https://api.github.com/users/scott 2>&1
# Expected: 304 Not Modified (if content hasn't changed)
```

**ETags save bandwidth** — the server doesn't send the body if it hasn't changed.

---

## Browser caching in practice

When you visit a website:

1. First visit: browser downloads all resources (HTML, CSS, JS, images)
2. Server includes `Cache-Control` and `ETag` headers
3. Second visit: browser checks its cache
   - If `max-age` hasn't expired → uses cached copy (no server request)
   - If expired → sends conditional request (`If-None-Match`)
   - Server responds `304` (use cache) or `200` (new content)

---

## Caching strategies

| Strategy | Cache-Control | Use case |
|----------|--------------|----------|
| No cache | `no-store` | Banking data, passwords |
| Validate always | `no-cache` | API responses, user profiles |
| Short cache | `public, max-age=60` | Trending data |
| Long cache | `public, max-age=31536000` | Static assets (images, CSS, JS) |
| Stale-while-revalidate | `max-age=3600, stale-while-revalidate=86400` | News articles |

---

## Cache busting

When you update a CSS file, users might have the old version cached. Solution: change the filename or add a version query parameter:

```html
<!-- Before update -->
<link href="style.css" rel="stylesheet">

<!-- After update — cache busted -->
<link href="style.css?v=2" rel="stylesheet">
<!-- Or: style.a1b2c3.css — hash in filename -->
```

---

## Senior mindset: "Cache invalidation is one of the two hard problems"

The two hard problems in computer science:
1. Naming things
2. Cache invalidation
3. Off-by-one errors

**Rule of thumb:** Be conservative with what you cache and aggressive with what you invalidate. Better to miss a cache hit than serve stale data.

---

## Summary

| Header | Purpose | Example |
|--------|---------|---------|
| `Cache-Control` | How to cache | `public, max-age=3600` |
| `ETag` | Version identifier | `"33a64df551425fcc55e"` |
| `If-None-Match` | Conditional request | Sends ETag back to server |
| `If-Modified-Since` | Date-based conditional | `Wed, 21 Oct 2025 07:28:00 GMT` |
| `304 Not Modified` | "Use your cache" | Response to conditional request |
