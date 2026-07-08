# DAY17: Checkpoint

1. Where does session data live in a server-side session architecture?

   <details>
   <summary>Answer</summary>
   On the **server** (in memory, Redis, or a database). The client only has a session ID.
   </details>

2. How can you "logout" a user with server-side sessions?

   <details>
   <summary>Answer</summary>
   Delete the session from the server store. Even if the client still has the cookie, the session is invalid.
   </details>

3. What advantage do server-side sessions have over JWTs?

   <details>
   <summary>Answer</summary>
   **Immediate revocation** — you can delete a session from the store at any time, instantly blocking access.
   </details>

4. What advantage do JWTs have over server-side sessions?

   <details>
   <summary>Answer</summary>
   **No server-side storage needed** — works across microservices, mobile apps, and third-party clients without a shared session store.
   </details>

5. Why should you regenerate the session ID after login?

   <details>
   <summary>Answer</summary>
   To prevent **session fixation** attacks — where an attacker gives you a session ID they know, and you accidentally use it after logging in.
   </details>
