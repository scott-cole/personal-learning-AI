# DAY02: Drill — "Play with the verbs"

Use the free JSONPlaceholder API to practice all five HTTP methods.

## Setup

All examples use `https://jsonplaceholder.typicode.com/posts`. The API returns fake blog posts.

## Part 1 — GET

```bash
# Get all posts (200 records)
curl https://jsonplaceholder.typicode.com/posts

# Get a single post
curl https://jsonplaceholder.typicode.com/posts/1

# Get posts filtered by user
curl "https://jsonplaceholder.typicode.com/posts?userId=1"
```

## Part 2 — POST

```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"my post","body":"hello world","userId":1}'
```

What status code do you get? What does the response body look like?

## Part 3 — PUT

```bash
curl -X PUT https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"id":1,"title":"updated","body":"everything changed","userId":1}'
```

Compare the body with the POST response. What's the same? What's different?

## Part 4 — PATCH

```bash
curl -X PATCH https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"only this changed"}'
```

## Part 5 — DELETE

```bash
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

What status code does DELETE return?

## Deliverables

Run through all five methods. Write down:
1. The status code returned for each method
2. The Content-Type header returned by the API
3. What happens if you send GET with a body?

## Hints

<details>
<summary>Click for hints</summary>

- Add `-v` to see the full request/response
- For POST, expect `201 Created`
- For DELETE, expect `204 No Content` or `200 OK`
- JSONPlaceholder doesn't actually save your data — it fakes the response
- GET with a body: most servers ignore it
</details>
