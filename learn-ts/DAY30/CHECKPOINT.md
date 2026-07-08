# DAY30: Checkpoint

**Closed book.** No looking back.

1. What is CORS and why did you need it for this project?

2. What Vite config change lets the client proxy API requests to the server?

3. Why separate the `useTodos` logic into a custom hook?

4. What's the difference between optimistic updates and re-fetching?

5. If you were to deploy this app, what would the production architecture look like?

---

<details>
<summary>Answers</summary>

1. CORS (Cross-Origin Resource Sharing) is a browser security feature that blocks requests from one origin to another. Your client (localhost:5173) and server (localhost:3001) are different origins, so the server needs to include CORS headers to allow the browser to accept the response.
2. Add `server: { proxy: { "/api": "http://localhost:3001" } }` to `vite.config.ts`. This makes the Vite dev server forward `/api/*` requests to the Express server.
3. Encapsulation and reusability. The fetch logic, loading state, and error handling live in one place instead of being spread across components. Any component can use the hook.
4. Optimistic updates immediately update the UI before the server responds (fast UX but risks inconsistency on failure). Re-fetching waits for the server response then refreshes all data (consistent but slower).
5. A production deployment would typically have: a reverse proxy (Nginx) serving both the built React static files and proxying `/api` to the Node.js server, a proper database (PostgreSQL/SQLite), environment variables for configuration, HTTPS, and process management (PM2 or Docker).
</details>
