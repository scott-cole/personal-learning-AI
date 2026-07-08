# DAY29: Drill — "URL Shortener API"

Build the complete URL shortener API in `DAY29/`.

## Setup

```bash
cd ~/Dev/learn-ts/DAY29
npm init -y
npm install express zod
npm install -D typescript @types/node @types/express tsx
```

## File structure

```
DAY29/
  package.json
  tsconfig.json
  src/
    index.ts
    types.ts
    schemas.ts
    services/
      urlService.ts
    routes/
      shorten.ts
```

## Completion criteria

- [ ] `POST /shorten` validates URL with Zod, generates slug, stores it, returns 201
- [ ] `POST /shorten` returns 400 with field errors for invalid input
- [ ] `POST /shorten` handles duplicate URLs (optional: return existing slug)
- [ ] `GET /:slug` redirects to the original URL with 302
- [ ] `GET /:slug` returns 404 if slug not found
- [ ] `GET /stats/:slug` returns visit count
- [ ] Each visit increments a counter
- [ ] All routes handle errors gracefully

## Testing

```bash
# Start the server
npx tsx src/index.ts

# Shorten a URL
curl -X POST http://localhost:3000/shorten \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com/very/long/url"}'

# Response:
# {"slug":"aB3xK9","originalUrl":"https://example.com/very/long/url","visits":0,"createdAt":"..."}

# Visit the short URL (follow redirect with -L)
curl -L http://localhost:3000/aB3xK9

# Check stats
curl http://localhost:3000/stats/aB3xK9

# Response:
# {"slug":"aB3xK9","originalUrl":"https://example.com/very/long/url","visits":1,"createdAt":"..."}
```

## Hints

<details>
<summary>Click for hints</summary>

- Use `z.string().url()` for URL validation in Zod
- For redirect: `res.redirect(302, originalUrl)`
- Store ShortUrl objects in a `Map<string, ShortUrl>`
- `generateSlug()` should check for collisions (regenerate if slug exists)
- Track visits in the `visitSlug` function: increment counter, return the URL
- Add `express.json()` middleware for POST body parsing
</details>

## Bonus

Add a `DELETE /:slug` endpoint. Add rate limiting (express-rate-limit). Add a simple HTML form at `GET /` for shortening URLs from the browser.

## When you're done

Show me all your files and a curl session demonstrating each endpoint.
