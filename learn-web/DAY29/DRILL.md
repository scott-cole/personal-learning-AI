# DAY29: Drill — "Design and document"

Your mission: write the `API_DESIGN.md` file for the URL shortener.

## Requirements for API_DESIGN.md

Create a complete API specification. Here's a template to get you started:

```markdown
# URL Shortener API Design

## Base URL
`https://short.url/api`

## Authentication
- API key via `X-API-Key` header
- Generate key at `/api/keys`

## Rate Limiting
- Unauthenticated: 100 req/hour
- Authenticated: 1000 req/hour
- Headers: X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset

## Endpoints

### POST /shorten
Create a shortened URL.

**Request:**
```json
{
    "url": "https://example.com/long-url"
}
```

**Success Response (201):**
```json
{
    "id": "url_abc123",
    ...
}
```

**Errors:**
- 400: Missing url field
- 422: Invalid URL format
- 429: Rate limit exceeded

---

### GET /{code}
Redirect to the original URL.

...

---

### GET /urls
List all URLs for the authenticated user.

...

---

### GET /urls/{id}
Get stats for a specific URL.

...

---

### DELETE /urls/{id}
Delete a shortened URL.

...

---

### PUT /urls/{id}
Update the destination URL.

...

## Data Model

```
URL {
    id: string (primary key)
    short_code: string (unique, 6-8 chars)
    long_url: string
    user_id: string (foreign key)
    clicks: integer
    created_at: timestamp
    updated_at: timestamp
}

User {
    id: string
    api_key: string
    email: string
    created_at: timestamp
}
```

## Error Response Format

```json
{
    "error": {
        "code": "INVALID_URL",
        "message": "The provided URL is not valid",
        "details": {}
    }
}
```

## CORS Policy

- Allow origin: `https://short.url`
- Methods: GET, POST, PUT, DELETE
- Headers: Content-Type, X-API-Key
```

## Fill in the details

Complete every section. For each endpoint, specify:
- Method and path
- Request body (if any)
- Success response (status code + body)
- Error responses (status codes)

## Think about edge cases

Answer these questions in your design:

1. What happens if someone shortens a URL that's already been shortened?
2. What if someone tries to access a deleted short URL?
3. How do you prevent users from shortening malicious URLs (phishing)?
4. Should the redirect be 301 (permanent) or 302 (temporary)? Why?
5. How long should short URLs live? Forever? Expire after 1 year?

## Bonus

Add an endpoint that wasn't in the requirements but would be useful:
- Bulk create
- QR code generation
- Custom short codes
- Expiration dates

## Deliverables

A complete `API_DESIGN.md` file with:
- All endpoints documented
- Request/response schemas
- Status codes
- Auth and rate limiting
- Data model
- CORS policy
- Edge case handling

## Hints

<details>
<summary>Click for hints</summary>

- 301 redirects are cached by browsers — if you need to update the URL later, use 302
- Google uses `goo.gl`, bit.ly uses `bit.ly` — the short domain should be unrelated to the original
- Random 7-char base62 strings give 62^7 ≈ 3.5 trillion combinations
- Always validate URLs — prevent `javascript:` and `data:` schemes
- For this project, in-memory storage is fine (the next day builds it)
</details>
