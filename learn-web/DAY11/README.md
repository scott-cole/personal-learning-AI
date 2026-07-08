# DAY11: API Versioning, Pagination, and Filtering

---

## The anecdote: "Versioning is textbook editions"

Every year, textbooks get updated: Edition 1, Edition 2, Edition 3.

| Textbook edition | API version |
|-----------------|-------------|
| Edition 1 (1995) | `/v1/` |
| Edition 2 (2000) | `/v2/` |
| Edition 3 (2010) | `/v3/` |

Students can keep using Edition 2 even though Edition 3 exists. They switch when they're ready.

APIs work the same way. You release `/v1/`, then later `/v2/` with breaking changes. Old clients keep working on `/v1/` while new clients use `/v2/`.

---

## Where to put the version

```bash
# In the URL path (most common)
curl https://api.example.com/v1/users
curl https://api.example.com/v2/users

# As a header (less common, more REST-pure)
curl -H "Accept: application/vnd.example.v1+json" https://api.example.com/users

# As a query parameter (least common)
curl https://api.example.com/users?v=1
```

**Best practice:** Put the version in the URL path. It's the simplest, most visible, and easiest to route.

---

## Pagination — "I can't show you everything at once"

When a collection has thousands of items, you don't send them all at once. You send pages.

```bash
# Request page 2, 10 items per page
curl "https://api.github.com/search/repositories?q=golang&page=2&per_page=10"
```

**Common pagination styles:**

| Style | How it works | Example |
|-------|-------------|---------|
| Page-based | `?page=2&per_page=10` | Simple, but pages shift if items are inserted/deleted |
| Cursor-based | `?cursor=abc123&limit=10` | Stable, but harder to implement |
| Offset-based | `?offset=20&limit=10` | Like page-based, uses offset instead of page number |

**Response should include pagination metadata:**

```json
{
    "data": [...],
    "pagination": {
        "page": 2,
        "per_page": 10,
        "total": 142,
        "total_pages": 15,
        "next": "https://api.example.com/users?page=3&per_page=10",
        "prev": "https://api.example.com/users?page=1&per_page=10"
    }
}
```

---

## Filtering — "Show me just what I need"

```bash
# Basic filtering
curl "https://api.github.com/repos/golang/go/issues?state=open"

# Multiple filters
curl "https://api.github.com/repos/golang/go/issues?state=open&labels=bug&milestone=42"

# Search-like filtering
curl "https://api.github.com/search/issues?q=is:open+label:bug+repo:golang/go"
```

**Common filter styles:**

| Style | Example | Use case |
|-------|---------|----------|
| Exact match | `?status=active` | Simple equality |
| Multiple values | `?status=active&status=pending` | OR conditions |
| Range | `?created_after=2024-01-01` | Date ranges |
| Partial match | `?q=search+term` | Full-text search |
| Nested | `?filter[price][gt]=10` | Complex filtering |

---

## Sorting — "Order matters"

```bash
# Simple sort
curl "https://api.github.com/search/repositories?q=golang&sort=stars"

# Sort with direction
curl "https://api.github.com/search/repositories?q=golang&sort=stars&order=desc"

# Multiple sort fields (API-specific syntax)
curl "https://api.example.com/users?sort=-created_at,name"
# -created_at = descending, name = ascending
```

---

## Field selection — "I only need these fields"

```bash
# GraphQL-style field selection
curl "https://api.example.com/users/42?fields=id,name,email"

# Sparse fieldsets (JSON:API style)
curl "https://api.example.com/users/42?fields[users]=id,name,email"
```

---

## Senior mindset: "Default pagination is your rate limit safety net"

Always paginate by default — even if the current dataset is small. A query that returns 50 rows today might return 50,000 in a year.

**Defaults:**
- `per_page`: 10-25 (reasonable default)
- `max_per_page`: 100 (hard limit)
- Default sort: newest first (or by ID descending)

---

## Summary

| Feature | Query pattern | Example |
|---------|--------------|---------|
| Versioning | `/v1/`, `/v2/` | URL path prefix |
| Pagination | `?page=2&per_page=10` | Page-based |
| Filtering | `?state=open&labels=bug` | Key-value filters |
| Sorting | `?sort=field&order=desc` | Sort by field |
| Fields | `?fields=id,name` | Sparse fieldset |
