# DAY20: Checkpoint

1. What does the "S" in HTTPS stand for?

   <details>
   <summary>Answer</summary>
   **Secure** — HTTP over TLS.
   </details>

2. What happens during the TLS handshake?

   <details>
   <summary>Answer</summary>
   Client and server agree on a cipher, exchange certificates, establish a shared encryption key, and verify the connection — all before any HTTP data is sent.
   </details>

3. Why can't you just use a self-signed certificate in production?

   <details>
   <summary>Answer</summary>
   Browsers don't trust self-signed certs — they show a security warning. Production certs need to be signed by a trusted **Certificate Authority**.
   </details>

4. What does `curl -k` do?

   <details>
   <summary>Answer</summary>
   **Skips TLS certificate verification** — allows connecting to servers with self-signed or invalid certs. Dangerous in production.
   </details>

5. How does the client verify the server's certificate?

   <details>
   <summary>Answer</summary>
   It checks the certificate's signature against its list of trusted **Certificate Authorities**, verifies the domain matches, and confirms the cert isn't expired.
   </details>
