# DAY27: CDN — Content Delivery Networks

---

## The anecdote: "A CDN is a chain of local warehouses"

Imagine you run an online store with one central warehouse in New York:

| Without CDN | With CDN |
|-------------|----------|
| Customer in Tokyo orders → ships from New York → arrives in 14 days | Customer in Tokyo orders → ships from Tokyo warehouse → arrives in 2 days |
| Customer in London orders → ships from New York → arrives in 10 days | Customer in London orders → ships from London warehouse → arrives in 1 day |
| Every order ships from one location | 50 warehouses worldwide |

A **CDN** (Content Delivery Network) is a network of servers distributed across the globe. Each server (called a **PoP** — Point of Presence) caches your content and serves it to users in that region.

---

## How a CDN works

```
User in Tokyo                   CDN PoP (Tokyo)              Origin Server (New York)
  |                                |                             |
  |--- GET /image.png ----------->|                             |
  |                                |  Cache miss?                |
  |                                |--- GET /image.png --------->|
  |                                |<--- image.png + Cache-Control
  |                                |  Store in edge cache        |
  |<--- image.png (from Tokyo) ---|                             |
  |                                |                             |
  |--- GET /image.png (2nd user)->|                             |
  |<--- image.png (from cache) ---|  Cache hit!                 |
```

First user: slow (cache miss). Second user: fast (cache hit). After that, everyone in Tokyo gets the cached version.

---

## Benefits of CDN

| Benefit | Why |
|---------|-----|
| **Speed** | Content is served from the nearest PoP — shorter distance = faster load |
| **Scale** | CDN handles millions of concurrent users — your origin server only needs to handle a fraction |
| **DDoS protection** | CDN absorbs large attacks before they reach your origin |
| **Availability** | If one PoP goes down, traffic routes to the next closest |
| **Cost savings** | Less bandwidth on your origin servers |

---

## What gets cached on a CDN

| Cached well | Not cached |
|-------------|------------|
| Static assets (CSS, JS, images) | Personalized content (user profiles) |
| Fonts, videos, audio files | API responses with auth |
| HTML pages (if not personalized) | Dynamic data (stock prices) |
| PDFs, downloads | Anything with `Cache-Control: private` or `no-store` |

---

## CDN configuration

Most CDNs are configured via a dashboard or API:

```text
# Cloudflare example — page rule
URL: https://example.com/assets/*
Cache Level: Standard
Edge Cache TTL: 7 days
```

Or via origin headers:

```text
# Your origin server tells the CDN what to cache
Cache-Control: public, max-age=604800, s-maxage=86400
```

`s-maxage` = proxy/CDN cache time. `max-age` = browser cache time.

CDNs respect your `Cache-Control` headers. They cache what you tell them to cache.

---

## CDN cache invalidation

When you update content, you need to tell the CDN to clear its cache:

```bash
# Cloudflare API purge
curl -X POST "https://api.cloudflare.com/client/v4/zones/ZONE_ID/purge_cache" \
  -H "Authorization: Bearer TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"files":["https://example.com/style.css"]}'

# Purge everything
curl -X POST ... -d '{"purge_everything":true}'
```

**Cost:** CDN purges are usually free but count-limited. Better approach: cache busting (change filename with hash).

---

## Go behind a CDN

Your Go app needs to tell the CDN what to cache:

```go
func handler(w http.ResponseWriter, r *http.Request) {
    // This response can be cached by CDN for 1 hour
    w.Header().Set("Cache-Control", "public, max-age=3600, s-maxage=3600")
    // ...
}

func userProfileHandler(w http.ResponseWriter, r *http.Request) {
    // This should NOT be cached (personalized)
    w.Header().Set("Cache-Control", "private, no-cache")
    // ...
}
```

If your Go app is behind a CDN, the CDN will see the client's real IP:

```go
func getClientIP(r *http.Request) string {
    // CDNs set CF-Connecting-IP (Cloudflare) or X-Forwarded-For
    if cfip := r.Header.Get("CF-Connecting-IP"); cfip != "" {
        return cfip
    }
    if fwd := r.Header.Get("X-Forwarded-For"); fwd != "" {
        return strings.Split(fwd, ",")[0]
    }
    return r.RemoteAddr
}
```

---

## Popular CDN providers

| Provider | Free tier | Notes |
|----------|-----------|-------|
| Cloudflare | Yes | Most popular, also provides DNS + DDoS protection |
| Fastly | No | High-performance, programmable VCL |
| Akamai | No | Enterprise, massive scale |
| AWS CloudFront | 1TB free | Tightly integrated with AWS |
| Bunny CDN | Very cheap | Simple, affordable |

---

## Senior mindset: "CDN is not a caching bandage — design for it"

Don't just slap a CDN on a slow site. Design your application to be cacheable:

1. Separate static and dynamic resources
2. Set appropriate `Cache-Control` headers
3. Use cache busting (hash in filenames) for static assets
4. Personalize via JavaScript, not server-rendered HTML (so the HTML page is cacheable)
5. Use CDN purge API sparingly — prefer cache busting

---

## Summary

| Concept | Description |
|---------|-------------|
| PoP | Point of Presence — a CDN server location |
| Edge cache | CDN server that caches your content |
| Origin | Your server (the source of truth) |
| Cache hit | Content served from CDN (fast) |
| Cache miss | CDN had to fetch from origin (slow for first user) |
| s-maxage | CDN/proxy cache TTL |
| Purge | Clear the CDN cache for specific URLs |
