# DAY06: Drill — "Check what version you're using"

Use curl and browser DevTools to see HTTP versions in the wild.

## Part 1 — Check HTTP/1.1 vs HTTP/2 with curl

```bash
# Force HTTP/1.1
curl -v --http1.1 https://www.example.com 2>&1 | grep "HTTP/"
# Look for: HTTP/1.1 200 OK

# Use HTTP/2 (preferred)
curl -v --http2 https://www.example.com 2>&1 | grep "HTTP/"
# Look for: HTTP/2 200
```

## Part 2 — Try HTTP/3

```bash
# See if the server supports HTTP/3
curl -v --http3 https://www.google.com 2>&1 | grep -E "(HTTP/|alpn|h3)"
```

If curl doesn't support `--http3`, you may need to update it.

## Part 3 — Browser DevTools

1. Open Chrome → DevTools → Network tab
2. Refresh a page (any busy site like `github.com`)
3. Right-click the column headers and enable **Protocol** column
4. Look at the protocol column — you'll see `h2` (HTTP/2) or `h3` (HTTP/3)

## Part 4 — See the difference

Time how long each version takes:

```bash
time curl --http1.1 -s -o /dev/null https://www.example.com
time curl --http2 -s -o /dev/null https://www.example.com
```

On a site with many assets, HTTP/2 is significantly faster.

## Deliverables

1. What protocol does `example.com` use?
2. What protocol does `google.com` use?
3. How many requests use `h2` vs `h3` on your favourite site?

## Hints

<details>
<summary>Click for hints</summary>

- `curl --http3` requires a curl build with HTTP/3 support (`brew install curl` with quiche)
- Most sites still support HTTP/1.1 as a fallback
- The `Protocol` column in DevTools shows `h2`, `h3`, or `http/1.1`
</details>
