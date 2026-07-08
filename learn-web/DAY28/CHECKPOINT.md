# DAY28: Checkpoint

1. What does XSS stand for and what does it enable an attacker to do?

   <details>
   <summary>Answer</summary>
   **Cross-Site Scripting** — injects malicious JavaScript into a page that other users view, allowing cookie theft, defacement, or phishing.
   </details>

2. How does a CSRF attack work?

   <details>
   <summary>Answer</summary>
   An attacker tricks a user into making an **unintended request** on a site where they're authenticated (e.g., via an `<img>` tag or hidden form submission).
   </details>

3. What's the best prevention for SQL injection?

   <details>
   <summary>Answer</summary>
   **Parameterized queries** (prepared statements) — never concatenate user input into SQL strings.
   </details>

4. What does the `Content-Security-Policy` header do?

   <details>
   <summary>Answer</summary>
   It tells the browser which sources of content (scripts, styles, images) are allowed to load, preventing XSS even if a script tag is injected.
   </details>

5. Why is "defence in depth" important for security?

   <details>
   <summary>Answer</summary>
   No single measure is perfect. Multiple **layers** of security ensure that if one is bypassed, others still protect the system.
   </details>
