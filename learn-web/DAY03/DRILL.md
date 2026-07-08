# DAY03: Drill — "Status code bingo"

Use `curl` to trigger as many different status codes as you can.

## The exercises

```bash
# 200 — Standard success
curl -v https://example.com

# 301 — Permanent redirect (HTTP → HTTPS)
curl -v http://example.com

# 404 — Not found
curl -v https://example.com/nonexistent

# 500 — Server error (use a test API)
curl -v https://httpbin.org/status/500

# 401 — Unauthorized (try accessing a protected resource)
curl -v https://httpbin.org/basic-auth/user/pass

# 403 — Forbidden
curl -v https://httpbin.org/status/403

# 405 — Method not allowed
curl -v -X DELETE https://example.com

# 429 — Rate limited (hit it fast multiple times)
for i in {1..20}; do curl -s -o /dev/null -w "%{http_code}\n" https://httpbin.org/status/429; done
```

## Your mission

1. Find at least **6 different status codes**
2. For each one, write down:
   - The code
   - What you think caused it
   - Whether it's a 1xx/2xx/3xx/4xx/5xx

## Bonus

Use `curl -w "%{http_code}"` to print **only** the status code:

```bash
curl -s -o /dev/null -w "%{http_code}" https://example.com
```

This is useful in scripts — you don't care about the body, just the status.

## Deliverables

List the 6+ status codes you found and what category each belongs to.

## Hints

<details>
<summary>Click for hints</summary>

- `https://httpbin.org/status/CODE` returns any status code you want
- Add `-v` when you want to see headers, remove it when you just want the body
- `-s` silences the progress meter, `-o /dev/null` discards the body
- `-w "%{http_code}"` is a curl write-out format — super useful
</details>
