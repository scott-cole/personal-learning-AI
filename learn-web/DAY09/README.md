# DAY09: RESTful API Design Principles

---

## The anecdote: "REST is a well-organised library"

Think of a public library:

| Library concept | REST equivalent |
|----------------|-----------------|
| The building itself | The API base URL (`https://api.example.com`) |
| Sections (Fiction, Non-fiction, Children's) | **Resources** (`/books`, `/authors`, `/users`) |
| A specific book on a shelf | A **specific resource** (`/books/42`) |
| The Dewey Decimal System | The **URL structure** |
| A library card | **Authentication** |
| "Only 3 books per person" | **Rate limiting** |
| A reference desk for complex queries | **Query parameters** |

REST (Representational State Transfer) is a set of design principles — not a standard, not a protocol. It's a way of organising your API so that it's predictable, consistent, and easy to use.

---

## Resource-oriented URLs

Everything is a **resource** (noun). URLs should be nouns, not verbs.

```
✅ GOOD (nouns):              ❌ BAD (verbs):
GET /users                    GET /getUsers
GET /users/42                 GET /getUser?id=42
POST /users                   POST /createUser
DELETE /users/42              GET /deleteUser?id=42
GET /users/42/orders          GET /getUserOrders?user=42
```

**Rules:**
- Plural nouns for collections: `/users`, `/products`, `/orders`
- ID for a specific resource: `/users/42`
- Nested for relationships: `/users/42/orders`
- No verbs in URLs (the HTTP method is the verb)

---

## HTTP methods as CRUD

| Action | SQL | HTTP method | URL |
|--------|-----|-------------|-----|
| Create | INSERT | POST | `/users` |
| Read | SELECT | GET | `/users`, `/users/42` |
| Replace | UPDATE (full) | PUT | `/users/42` |
| Update | UPDATE (partial) | PATCH | `/users/42` |
| Delete | DELETE | DELETE | `/users/42` |

```bash
# List all users
curl https://api.example.com/users

# Get user 42
curl https://api.example.com/users/42

# Create a user
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Scott","email":"scott@example.com"}'

# Update user 42
curl -X PATCH https://api.example.com/users/42 \
  -H "Content-Type: application/json" \
  -d '{"email":"newemail@example.com"}'

# Delete user 42
curl -X DELETE https://api.example.com/users/42
```

---

## Consistent status codes

| Code | When |
|------|------|
| 200 | GET success, PUT success |
| 201 | POST (resource created) |
| 204 | DELETE success (no body) |
| 400 | Bad request (validation error) |
| 401 | Unauthenticated (not logged in) |
| 403 | Unauthorized (not allowed) |
| 404 | Resource not found |
| 409 | Conflict (duplicate, version mismatch) |
| 422 | Unprocessable entity (validation) |
| 429 | Rate limited |
| 500 | Server error |
| 503 | Service unavailable |

---

## Consistent error responses

Every error should return the same format:

```json
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Email is required",
        "details": {
            "field": "email",
            "reason": "missing"
        }
    }
}
```

---

## HATEOAS (optional, but nice)

Hypermedia As The Engine Of Application State. Include links in responses so clients can navigate:

```json
{
    "id": 42,
    "name": "Scott",
    "_links": {
        "self": { "href": "/users/42" },
        "orders": { "href": "/users/42/orders" },
        "friends": { "href": "/users/42/friends" }
    }
}
```

---

## Senior mindset: "Consistency beats cleverness"

A REST API is a contract. The most important quality is **consistency**.

- If POST `/users` returns 201, POST `/products` should also return 201
- If errors are `{"error":{"message":"..."}}` in one endpoint, they should be that format everywhere
- If dates are ISO 8601 in one response, they should be ISO 8601 everywhere

Developers using your API should be able to guess how it works without reading the docs.

---

## Summary

| Principle | Rule |
|-----------|------|
| Resources are nouns | `/users`, not `/getUsers` |
| HTTP methods are verbs | GET = read, POST = create, etc. |
| Consistent status codes | 200 success, 201 created, 404 not found |
| Consistent errors | Same error format everywhere |
| Version your API | `/v1/users`, `/v2/users` |
| Use JSON | Request and response bodies in JSON |
