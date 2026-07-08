# DAY24: Drill — "X-ray a website"

Use DevTools and HTTPie to dissect a website's network traffic.

## Part 1 — Install HTTPie and compare

```bash
brew install httpie

# Compare the output
curl -s https://api.github.com/users/scott | head -3
http https://api.github.com/users/scott 2>&1 | head -10
```

## Part 2 — Send a POST with HTTPie

```bash
http POST https://jsonplaceholder.typicode.com/posts \
  title="Hello" body="World" userId:=1

# Compare with curl:
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"Hello","body":"World","userId":1}'
```

Which is easier to read?

## Part 3 — DevTools Network tab deep dive

1. Open DevTools → Network tab
2. Visit `https://example.com`
3. Click the first request (the HTML document)
4. Explore:

   **Headers tab:**
   - What was the request method?
   - What was the response status?
   - What's the Content-Type?

   **Timing tab:**
   - How long did DNS take?
   - How long was TTFB (Waiting)?
   - What phase took the longest?

## Part 4 — Analyse a complex site

Visit any modern website (github.com, youtube.com, etc.) with the Network tab open.

Answer:
1. How many requests were made?
2. How much total data was transferred?
3. What was the slowest single request?
4. What type of resources take the longest? (images? JS? API calls?)

## Part 5 — Compare with and without cache

1. Load a site first time (fresh cache)
2. Note the total load time
3. Reload (cached)
4. Compare the difference in time and number of requests

## Part 6 — Use DevTools to debug a failed request

Create a broken request and diagnose it:

```javascript
// In DevTools Console, run:
fetch('https://httpbin.org/status/500')
  .then(r => {
    if (!r.ok) throw new Error(`HTTP ${r.status}`)
    return r.json()
  })
  .catch(e => console.error('Error:', e.message))
```

Look at the Network tab — what status code did it return? What was the response body?

## Deliverables

1. HTTPie vs curl comparison (which do you prefer?)
2. The Timing breakdown for `example.com`
3. Total requests and data for your favourite site
4. The 500 error response in DevTools

## Hints

<details>
<summary>Click for hints</summary>

- HTTPie uses `key=value` for string fields and `key:=value` for JSON numbers/booleans
- The "Waterfall" column shows request timing visually — wider = slower
- TTFB includes the server's processing time
- The "Size" column shows both the actual body size and the "transferred" size (headers + compressed body)
- Use the "Preserve log" checkbox to keep requests across page loads
</details>
