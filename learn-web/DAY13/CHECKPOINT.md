# DAY13: Checkpoint

1. What are the three parts of a JWT, separated by dots?

   <details>
   <summary>Answer</summary>
   **Header.Payload.Signature**
   </details>

2. Can anyone read the payload of a JWT without the secret?

   <details>
   <summary>Answer</summary>
   Yes — the payload is only base64-encoded, not encrypted. The signature only prevents **tampering**.
   </details>

3. What does the `exp` claim in a JWT mean?

   <details>
   <summary>Answer</summary>
   **Expiration time** — the token is not valid after this Unix timestamp.
   </details>

4. How does a client send a JWT to a server?

   <details>
   <summary>Answer</summary>
   In the `Authorization` header: `Authorization: Bearer <jwt>`
   </details>

5. What's a disadvantage of JWTs compared to server-side sessions?

   <details>
   <summary>Answer</summary>
   JWTs can't be **revoked** early (until they expire). If one is stolen, it's usable for its entire lifetime.
   </details>
