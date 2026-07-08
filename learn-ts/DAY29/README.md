# DAY29: Final Project — URL Shortener API

---

## "The URL shortener is a phone book"

You meet someone at a conference. They hand you a business card with a long, impossible-to-remember URL. You'd never type it. Instead, you hand them your card with a short code: `exm.pl/abc123`.

A URL shortener is a phone book that maps short codes to long URLs. Give it a short code, it looks up the long URL and redirects you there.

---

## What we're building

A RESTful API with Express + TypeScript:

```
POST   /shorten    { url: "https://verylongurl.com/..." } → { slug: "abc123", url: "..." }
GET    /:slug      → 302 Redirect to original URL
GET    /stats/:slug → { slug, url, visits, createdAt }
```

---

## Architecture

```
src/
  index.ts          — Express app entry
  routes/
    shorten.ts      — POST /shorten, GET /:slug, GET /stats/:slug
  services/
    urlService.ts   — business logic (create, find, track)
  schemas.ts        — Zod validation schemas
  types.ts          — interfaces
```

---

## Data model

```typescript
interface ShortUrl {
  slug: string;
  originalUrl: string;
  visits: number;
  createdAt: string;
}
```

In-memory store (Map<string, ShortUrl>) is fine for this project.

---

## API specification

### POST /shorten
```json
// Request:
{ "url": "https://example.com/very/long/url/that/needs/shortening" }

// Response (201):
{ "slug": "aB3xK9", "originalUrl": "https://...", "visits": 0, "createdAt": "2024-..." }

// Error (400):
{ "error": "Invalid URL" }
```

### GET /:slug
```
302 Found → redirects to original URL
404 → { "error": "Short URL not found" }
```

### GET /stats/:slug
```json
// Response:
{ "slug": "aB3xK9", "originalUrl": "https://...", "visits": 5, "createdAt": "2024-..." }
```

---

## Key TypeScript features used

| Feature | Where |
|---------|-------|
| Interfaces | ShortUrl type, request/response shapes |
| Generics | Repository pattern from DAY21 |
| Zod validation | POST body validation |
| Utility types | Omit for create payload |
| Async/await | Route handlers |
| Discriminated unions | Error responses |

---

## Generating slugs

```typescript
function generateSlug(length = 6): string {
  const chars = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";
  let slug = "";
  for (let i = 0; i < length; i++) {
    slug += chars[Math.floor(Math.random() * chars.length)];
  }
  return slug;
}
```

Your turn. Open DAY29/DRILL.md.
