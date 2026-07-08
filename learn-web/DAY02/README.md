# DAY02: HTTP Methods — GET, POST, PUT, PATCH, DELETE

---

## The anecdote: "HTTP methods are verbs in a recipe book"

You have a recipe book. Each recipe is a resource. There are five things you might do:

| Action | Recipe book equivalent | HTTP method |
|--------|----------------------|-------------|
| Read a recipe | Open the book and look | **GET** |
| Add a new recipe | Write a new page | **POST** |
| Replace a recipe | Tear out a page and rewrite it | **PUT** |
| Update a recipe | Fix a typo on one line | **PATCH** |
| Remove a recipe | Tear out the page | **DELETE** |

Each method has a specific meaning. The server uses the method to decide *what to do* with the request.

---

## GET — Read something

GET is the most common method. It says "give me this resource." It should **never** change anything on the server.

```bash
curl https://api.github.com/users/scott
```

This fetches user data. Run it 100 times — the data won't change (unless Scott edits his profile).

**Key rules:**
- GET requests have **no body**
- GET requests should be **idempotent** (safe to repeat)
- GET requests can be **cached** by browsers and proxies
- GET parameters go in the URL query string: `?name=value`

---

## POST — Create something

POST says "here's data, please create a new resource."

```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"foo","body":"bar","userId":1}'
```

**Key rules:**
- POST sends data in the **body**
- POST is **not idempotent** — sending it twice creates two resources
- POST usually returns `201 Created`

---

## PUT — Replace something

PUT says "set this resource to exactly this data." If the resource doesn't exist, create it. If it does, replace it entirely.

```bash
curl -X PUT https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"id":1,"title":"replaced","body":"completely new","userId":1}'
```

**Key rules:**
- PUT is **idempotent** — same PUT 10 times = same result
- PUT replaces the **entire** resource
- Missing fields are removed (or set to defaults)

---

## PATCH — Partially update something

PATCH says "change just these fields." It's a partial update.

```bash
curl -X PATCH https://jsonplaceholder.typicode.com/posts/1 \
  -H "Content-Type: application/json" \
  -d '{"title":"only the title changed"}'
```

**Key rules:**
- PATCH only changes the fields you send
- PATCH can be idempotent (depends on the operation)
- PATCH is newer than PUT (added in RFC 5789 in 2010)

---

## DELETE — Remove something

DELETE says "remove this resource."

```bash
curl -X DELETE https://jsonplaceholder.typicode.com/posts/1
```

**Key rules:**
- DELETE is **idempotent** — deleting something twice is still deleted
- DELETE usually returns `204 No Content` (success, nothing to show)
- Some APIs return `200 OK` with a confirmation message

---

## The full picture

```
Method   | Idempotent? | Has body? | Use case
---------|-------------|-----------|---------
GET      | Yes         | No        | Reading data
POST     | No          | Yes       | Creating data
PUT      | Yes         | Yes       | Replacing data
PATCH    | Depends     | Yes       | Updating data
DELETE   | Yes         | No*       | Removing data
```

*Some DELETE requests have bodies, but most don't.

---

## Senior mindset: "POST is a factory, PUT is a copy-assign"

Think of POST as "make a new one, you decide the ID." PUT as "put this data at this exact address."

**Real world:** Most APIs use POST for creation and PUT for replacement. But some APIs use only POST and PUT and skip PATCH entirely. Read the docs for the API you're using — don't assume.

---

## Summary

| Method | Meaning | curl flag |
|--------|---------|-----------|
| GET | Read a resource | `curl URL` (default) |
| POST | Create a resource | `curl -X POST URL -d 'data'` |
| PUT | Replace a resource | `curl -X PUT URL -d 'data'` |
| PATCH | Update part of a resource | `curl -X PATCH URL -d 'data'` |
| DELETE | Remove a resource | `curl -X DELETE URL` |
