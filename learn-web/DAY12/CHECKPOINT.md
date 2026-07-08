# DAY12: Checkpoint

1. Is Basic Auth secure over plain HTTP?

   <details>
   <summary>Answer</summary>
   No — the password is only base64-encoded, not encrypted. Anyone on the network can decode it.
   </details>

2. What's the difference between authentication and authorisation?

   <details>
   <summary>Answer</summary>
   Auth**entication** = who you are. Auth**orisation** = what you're allowed to do.
   </details>

3. You see `Authorization: Basic YWRtaW46c2VjcmV0`. What are the credentials?

   <details>
   <summary>Answer</summary>
   `admin:secret` (base64 decode of `YWRtaW46c2VjcmV0`)
   </details>

4. Why should API keys be long random strings instead of short tokens?

   <details>
   <summary>Answer</summary>
   To prevent brute-force guessing. A 32-byte hex string (64 chars) has 2^256 possible values.
   </details>

5. An API client sends `Authorization: Bearer abc123` and gets 403 Forbidden. What's the most likely reason?

   <details>
   <summary>Answer</summary>
   Authentication succeeded (the token is valid) but **authorisation** failed — the user doesn't have permission for that action.
   </details>
