# DAY04: Drill — "Read the envelope"

Use `curl -v` to inspect headers and send custom ones.

## Part 1 — Observe response headers

```bash
curl -v https://jsonplaceholder.typicode.com/posts/1
```

Write down every response header you see. What is the `Content-Type`?

## Part 2 — Send a custom header

```bash
curl -v -H "X-Debug: true" -H "Accept-Language: fr" https://jsonplaceholder.typicode.com/posts/1
```

Look at the request headers. Can you see your custom header?

## Part 3 — Negotiate content type

Some APIs return different formats based on the `Accept` header:

```bash
# Ask for XML (if the API supports it)
curl -H "Accept: application/xml" https://jsonplaceholder.typicode.com/posts/1

# Ask for JSON explicitly
curl -H "Accept: application/json" https://jsonplaceholder.typicode.com/posts/1
```

Does the API return different content for different Accept headers?

## Part 4 — Send data with content type

```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"test","body":"hello","userId":1}'

# Now try without Content-Type
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -d '{"title":"test","body":"hello","userId":1}'
```

What happens when you omit `Content-Type`?

## Part 5 — Content-Length

Use `-w` to see response size:

```bash
curl -s -o /dev/null -w "Size: %{size_download} bytes\nContent-Type: %{content_type}\n" \
  https://jsonplaceholder.typicode.com/posts/1
```

## Deliverables

1. List all response headers from Part 1
2. Did the API respond differently for different Accept headers?
3. What happened when you omitted `Content-Type` in Part 4?
4. How many bytes was the response in Part 5?

## Hints

<details>
<summary>Click for hints</summary>

- JSONPlaceholder always returns JSON regardless of Accept (most APIs do)
- Without Content-Type, curl uses `application/x-www-form-urlencoded` by default
- The `Content-Length` header tells you the body size before you download it
</details>
