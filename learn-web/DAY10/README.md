# DAY10: URL Structure — Path Parameters and Query Strings

---

## The anecdote: "URLs are a GPS address"

You give someone GPS directions to your favourite restaurant:

```
GPS: 123 Main Street, Downtown, London → Restaurant
```

A URL works the same way — it's a set of increasingly specific directions:

```
https://api.example.com/v2/ restaurants / 42 / reviews ? page = 2 & sort = newest
 \___/   \_____________/ \_/ \___________/ \_/ \______/ \_/ \____/ \_/ \____/
protocol    server         |   collection   |  item     |   key    |   value
                        version           identifier  query     parameter
```

Each part tells the server where to find what you want.

---

## Path parameters — resource identifiers

Path parameters identify **which** resource. They're part of the URL path, marked with `:` or `{}` in docs:

```
GET /users/:id
GET /users/{id}
```

In curl:

```bash
# Path parameter: user ID = 42
curl https://api.github.com/users/42

# Multiple path parameters
curl https://api.github.com/repos/golang/go/issues/1
#              /repos/:owner/:repo/issues/:number
```

**Rules:**
- Required (you can't GET a specific user without an ID)
- Part of the URL structure
- Should be URL-safe (no spaces, no special chars)

---

## Query parameters — filters and options

Query parameters **modify** the request. They come after `?` in `key=value` pairs separated by `&`:

```
?search=golang&page=2&limit=10&sort=stars&order=desc
```

```bash
# Filter by search term
curl "https://api.github.com/search/repositories?q=golang"

# Pagination
curl "https://api.github.com/search/repositories?q=golang&page=2&per_page=5"

# Sorting
curl "https://api.github.com/search/repositories?q=golang&sort=stars&order=desc"
```

**Rules:**
- Optional (URL works without them)
- Key-value pairs
- Multiple values can repeat: `?id=1&id=2`
- No spaces (use `%20` or `+`)

---

## URL encoding

Special characters must be **percent-encoded**:

| Character | Encoded | Why |
|-----------|---------|-----|
| space | `%20` or `+` | Not allowed in URLs |
| `?` | `%3F` | Reserved for query start |
| `&` | `%26` | Reserved for parameter separator |
| `#` | `%23` | Reserved for fragment |
| `/` | `%2F` | Reserved for path separator |

curl handles encoding for `-d` and `--data-urlencode`:

```bash
# curl auto-encodes
curl "https://example.com/search?q=go language"
# Actually sends: q=go%20language

# Use --data-urlencode for POST bodies
curl -X POST https://httpbin.org/post \
  --data-urlencode "comment=hello world & goodbye"
```

---

## Path vs query — when to use which

| Use path params for | Use query params for |
|--------------------|---------------------|
| Resource IDs | Filters |
| Resource relationships | Search terms |
| Required identifiers | Pagination |
| Version numbers | Sorting options |
| `/users/:id` | `?status=active&page=1` |

**Rule of thumb:** If it identifies a specific resource, it belongs in the path. If it modifies how the response is returned, it belongs in the query string.

---

## Senior mindset: "URLs should be human-readable"

A good URL tells you what it does just by looking at it:

```
❌ https://api.example.com/getItems?a=42&b=search&c=2
✅ https://api.example.com/users/42/orders?search=golang&page=2
```

Your grandmother should be able to guess what the second URL does.

---

## Summary

| Element | Syntax | Example | Purpose |
|---------|--------|---------|---------|
| Path param | `/:id` | `/users/42` | Identifies a resource |
| Query param | `?key=value` | `?page=2` | Filters, modifies |
| Fragment | `#section` | `#reviews` | Scrolls the page |
| Encoding | `%20` | `hello%20world` | Escapes special chars |
