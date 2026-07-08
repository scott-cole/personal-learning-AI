# DAY10: Drill — "Navigate by URL"

Use curl to explore path and query parameters on the GitHub API.

## Part 1 — Path parameters

```bash
# Get a specific user (path param)
curl https://api.github.com/users/google

# Get a specific repo (nested path params)
curl https://api.github.com/repos/golang/go

# Get issues for that repo (nested path params + collection)
curl https://api.github.com/repos/golang/go/issues
```

URL patterns:
```
/users/:username
/repos/:owner/:repo
/repos/:owner/:repo/issues
```

## Part 2 — Query parameters

```bash
# Search repos (query params)
curl "https://api.github.com/search/repositories?q=web+framework&sort=stars&order=desc"

# Paginate results
curl "https://api.github.com/search/repositories?q=web+framework&page=1&per_page=3"

# Filter issues by state
curl "https://api.github.com/repos/golang/go/issues?state=closed&labels=bug"
```

## Part 3 — Build URLs manually

Take these requirements and build the URL:

1. Get all **open** issues for the repo `golang/go` with **label** "documentation"
2. Search for repos with **topic** "http", **sort** by stars, **page** 2, **10 per page**
3. Get **user** "scott", then get their **repos**

Write the curl commands yourself.

## Part 4 — Encoding

```bash
# curl auto-encodes this:
curl "https://api.github.com/search/repositories?q=go language"
# Actually sends: q=go%20language

# Try with special characters:
curl --get --data-urlencode "q=go & http" \
  "https://api.github.com/search/repositories"
```

## Deliverables

1. The 3 curl commands from Part 3
2. What happens if you don't encode the space in a query parameter?
3. What's the difference between path `/users/42` and `/users/42/orders`?

## Hints

<details>
<summary>Click for hints</summary>

- Part 3.1: `?state=open&labels=documentation`
- Part 3.2: `?q=topic:http&sort=stars&page=2&per_page=10`
- `--get --data-urlencode` forces curl to use GET with URL-encoded data
- Spaces in URLs are technically illegal but many browsers auto-fix them
</details>
