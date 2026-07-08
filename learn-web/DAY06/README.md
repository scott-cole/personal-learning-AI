# DAY06: HTTP/1.1 vs HTTP/2 vs HTTP/3

---

## The anecdote: "HTTP versions are generations of delivery trucks"

In the 1990s, delivery vans (HTTP/1.1) worked like this:
- One driver, one package per trip
- Deliver the package, drive back to the warehouse, grab the next one
- If you needed CSS, JS, and an image? Three separate trips

Then came HTTP/2:
- Same van, but now it can carry 100 packages at once (multiplexing)
- The driver can prioritise: "deliver the CSS first, the tracking pixel last"
- One trip loads an entire page

Now HTTP/3 is here:
- The van is the same, but the roads are better (QUIC over UDP instead of TCP)
- No more traffic jams (head-of-line blocking)
- Faster starts (0-RTT connection setup)

---

## HTTP/1.1 — The original workhorse (1997)

Every request needs its own TCP connection (or at best, one request at a time per connection).

```
Browser                Server
  |------- Request 1 ------>|
  |<----- Response 1 -------|
  |------- Request 2 ------>|  ← must wait for response 1
  |<----- Response 2 -------|
  |------- Request 3 ------>|  ← must wait for response 2
  |<----- Response 3 -------|
```

**Workarounds:**
- **Keep-Alive** — reuse the same TCP connection for multiple requests (but still one at a time)
- **Domain sharding** — load resources from multiple domains (cdn1.example.com, cdn2.example.com)
- **Sprite sheets** — combine many images into one big image and crop with CSS

**Problem:** If one request is slow, everything behind it queues up. This is called **head-of-line blocking**.

---

## HTTP/2 — Multiplexing (2015)

One connection, many simultaneous requests.

```
Browser                Server
  |------- Request 1 ------>|
  |------- Request 2 ------>
  |------- Request 3 ------>
  |<----- Response 1 -------|
  |<----- Response 2 -------|
  |<----- Response 3 -------|  ← all happening simultaneously
```

**New features:**
- **Multiplexing** — multiple requests/responses on one connection
- **Header compression** — headers are compressed (HPACK), reducing overhead
- **Server push** — server sends resources before the client asks for them
- **Binary protocol** — not human-readable text anymore

**How to check:** Look for `x-http2` or `http2` in response headers, or use curl:

```bash
curl -v --http2 https://example.com
```

Most modern websites use HTTP/2. You don't need to do anything special as a developer — browsers support it automatically.

---

## HTTP/3 — QUIC-based (2022)

HTTP/3 runs on **QUIC** (Quick UDP Internet Connections) instead of TCP.

```
                    HTTP/3 (QUIC over UDP)
Browser ───────────────────────────────── Server
           No head-of-line blocking
           Faster connection setup (0-RTT)
           Built-in encryption
           Connection migration (switch WiFi → mobile without dropping)
```

**Key differences:**
- **UDP instead of TCP** — QUIC is built on UDP
- **No head-of-line blocking at transport layer** — if one packet is lost, only that stream waits
- **0-RTT handshake** — previously visited sites can send data immediately
- **Connection migration** — switch from WiFi to 5G without reconnecting

**Check if a site supports HTTP/3:**

```bash
curl -v --http3 https://www.google.com 2>&1 | grep "alpn"
# Look for "h3" in the ALPN negotiation
```

---

## Comparison table

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
|---------|----------|--------|--------|
| Transport | TCP | TCP | QUIC (UDP) |
| Multiplexing | No (one at a time) | Yes | Yes |
| Header compression | No | HPACK | QPACK |
| Server push | No | Yes | Yes |
| Encryption | Optional | Required (de facto) | Required |
| Connection setup | 3-way handshake | 3-way + TLS | 0-1 RTT |
| Head-of-line blocking | Yes | Transport-layer only | No |

---

## Senior mindset: "You don't choose HTTP/3, your infrastructure does"

As an app developer, you don't pick HTTP/3 — your CDN, reverse proxy, and load balancer do. Your code sends HTTP requests using a library. The library negotiates the best version available.

What you *should* do:
- Use HTTPS everywhere (HTTP/2 and HTTP/3 require it)
- Keep your server software up to date
- Let your infrastructure negotiate the protocol

---

## Summary

| Version | Year | Key innovation |
|---------|------|----------------|
| HTTP/1.1 | 1997 | Keep-Alive, chunked transfer |
| HTTP/2 | 2015 | Multiplexing, header compression |
| HTTP/3 | 2022 | QUIC, 0-RTT, no HoL blocking |
