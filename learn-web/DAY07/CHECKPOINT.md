# DAY07: Checkpoint

1. What does `curl -I` do?

   <details>
   <summary>Answer</summary>
   Fetches **only the response headers** (HEAD request), no body.
   </details>

2. You run `curl -d @data.json` and get an error. What's the most likely cause?

   <details>
   <summary>Answer</summary>
   The file `data.json` doesn't exist or the path is wrong. The `@` prefix reads from a file.
   </details>

3. What flag would you use to make curl follow a 301 redirect automatically?

   <details>
   <summary>Answer</summary>
   `-L` (or `--location`)
   </details>

4. You want to print just the HTTP status code in a script, discarding the body and progress output. What flags do you use?

   <details>
   <summary>Answer</summary>
   `curl -s -o /dev/null -w "%{http_code}" URL`
   </details>

5. What does `-k` or `--insecure` do? Why is it dangerous?

   <details>
   <summary>Answer</summary>
   It **skips TLS certificate verification**. Dangerous because it allows man-in-the-middle attacks.
   </details>
