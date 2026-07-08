# DAY24: HTTPie, cURL, and Browser DevTools Network Tab

---

## The anecdote: "DevTools is an X-ray machine for web traffic"

A mechanic looks at your car engine:

| Without X-ray | With X-ray |
|---------------|------------|
| Guess what's wrong | See exactly which part is broken |
| Replace random parts | Fix the specific issue |
| Takes hours | Takes minutes |

Browser DevTools (and HTTPie) are your X-ray machine for HTTP traffic. They show you exactly what's happening in every request.

---

## cURL vs HTTPie

Both send HTTP requests. The difference is **format and readability**.

```bash
# cURL — minimal but powerful
curl https://api.github.com/users/scott

# HTTPie — colourful, formatted output
http https://api.github.com/users/scott
```

| Feature | cURL | HTTPie |
|---------|------|--------|
| Installed | ✅ Built into macOS | ❌ `brew install httpie` |
| Output | Raw body | Syntax-highlighted, formatted JSON |
| Headers | `-v` flag | Shown by default |
| POST JSON | `-d '{}' -H "Content-Type: application/json"` | `http POST url key=value` |
| Scripting | ✅ Perfect | ⚠️ Good but harder to parse |
| Verbose mode | `-v` | `-v` |

```bash
# Install HTTPie
brew install httpie

# Compare with cURL
curl -v https://api.github.com/users/scott | head -5
http https://api.github.com/users/scott
```

---

## Browser DevTools — Network tab

The single most powerful tool for a web developer.

**How to open:**
- Chrome: `Cmd+Option+I` → Network tab
- Firefox: `Cmd+Option+I` → Network tab

**What you can see:**

| Column | What it shows |
|--------|---------------|
| Name | The resource URL |
| Status | HTTP status code |
| Type | Content type (document, script, stylesheet) |
| Size | Body size + overhead |
| Time | Total request duration |
| Waterfall | Visual timeline of the request |

**Click any request to see:**

| Tab | What it shows |
|-----|---------------|
| Headers | Request + response headers |
| Payload | Request body (POST data) |
| Response | Response body |
| Timing | DNS, TCP, TLS, request, response time breakdown |
| Cookies | Cookies sent and received |

---

## The Timing tab — your performance diagnosis tool

Every request is broken down into phases:

```
DNS Lookup:    5ms  →  Was DNS cached?
TCP Connect:  15ms  →  Network round trip
TLS Handshake: 20ms →  Certificate negotiation
Request Send:   1ms  →  Upload time
Waiting (TTFB): 200ms → Server processing time
Content Download: 10ms → Download time
```

**TTFB (Time To First Byte)** is the most important metric — how long until the server starts responding.

---

## Using DevTools to inspect a real page

1. Open DevTools → Network tab
2. Visit any website
3. Look at the waterfall — which request took the longest?
4. Click a slow request → Timing tab
5. Which phase was the bottleneck?

---

## The "Disable cache" checkbox

When developing, check **Disable cache** to force fresh requests every time. Without it, your changes might not show up because the browser serves cached files.

---

## Senior mindset: "The Network tab is your first debugging tool"

Before writing a single line of debugging code:

1. Open DevTools → Network
2. Reproduce the issue
3. Look at the failing request
4. Check the status code, headers, and response body
5. 90% of the time, the bug is obvious

---

## Summary

| Tool | Best for |
|------|----------|
| cURL | Scripting, quick testing, automation |
| HTTPie | Ad-hoc API exploration, readable output |
| DevTools Network | Full request analysis, waterfall timing, debugging |

| DevTools phase | What it measures |
|----------------|-----------------|
| Queueing | Waiting in the browser's request queue |
| DNS | Domain resolution time |
| TCP | Connection establishment |
| TLS | Encryption handshake |
| TTFB | Server processing time |
| Download | Response body transfer |
