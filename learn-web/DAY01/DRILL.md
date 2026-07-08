# DAY01: Drill — "Watch the handshake"

Your mission: observe a real HTTP conversation using `curl` and your browser's DevTools.

## Part 1 — curl

Run these commands one at a time and observe the output:

```bash
curl -v https://example.com
```

Look for:
- The **request line** (starts with `>`)
- The **response status line** (starts with `< HTTP/`)
- The **headers** (lines starting with `<`)
- The **body** (the HTML after the blank line)

Now try a URL that doesn't exist:

```bash
curl -v https://example.com/nonexistent
```

What status code do you get?

## Part 2 — Browser DevTools

1. Open Chrome or Firefox
2. Press `Cmd+Option+I` (Mac) or `F12` (Windows) to open DevTools
3. Click the **Network** tab
4. Refresh the page (Cmd+R)
5. Click on the first request (the document itself)

Look at the **Headers** tab. You'll see the exact same request and response headers that curl showed you.

## Part 3 — Break down a URL

Take this URL and identify each part:

```
https://www.googleapis.com/books/v1/volumes?q=go+programming&maxResults=5
```

Write down: scheme, host, path, query parameters.

## Deliverables

No code to submit. Just run the commands and observe. When you're done, tell me:
1. What status code did `https://example.com/nonexistent` return?
2. What's one header you saw in DevTools that you didn't see with curl?
3. Write down the parts of the Google Books URL above.

## Hints

<details>
<summary>Click for hints</summary>

- Status code 404 means "Not Found"
- Your browser probably sent 20+ requests on a single page (CSS, JS, images)
- The `Host` header tells the server which website you want (important for shared servers)
</details>
