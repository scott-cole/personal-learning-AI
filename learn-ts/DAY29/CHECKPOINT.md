# DAY29: Checkpoint

**Closed book.** No looking back.

1. What HTTP status code is used for a redirect, and what method sends it in Express?

2. Why validate URLs with Zod's `z.string().url()` instead of just checking `typeof url === "string"`?

3. How would you prevent two identical long URLs from creating different slugs?

4. What's the purpose of the visits counter in a URL shortener?

5. How does `res.redirect(302, url)` work from the browser's perspective?

---

<details>
<summary>Answers</summary>

1. 302 (Found). Express: `res.redirect(302, url)`.
2. TypeScript's `typeof` check only confirms it's a string at compile time, but at runtime any string (including `"not-a-url"`) could arrive. Zod's `.url()` validates the actual format against URL standards at runtime.
3. Check if the URL already exists in the store before creating a new slug. If it exists, return the existing slug instead of generating a new one.
4. Analytics — track how many times each short URL is accessed, which is useful for marketing campaigns and link tracking.
5. The browser receives a 302 response with a `Location` header pointing to the original URL. The browser automatically makes a second request to that URL and displays the page.
</details>
