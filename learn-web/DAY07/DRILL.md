# DAY07: Drill — "Master the phone"

Your mission: perform a series of curl challenges using the flags from today's lesson.

## Challenge 1 — Timing

Measure how long it takes to load `https://example.com`:

```bash
curl -w "Time: %{time_total}s\n" -s -o /dev/null https://example.com
```

Run it 5 times. Is the time consistent?

## Challenge 2 — Headers only

Fetch **only the headers** of `https://github.com`:

```bash
curl -I https://github.com
```

What's the first header in the response?

## Challenge 3 — Follow the redirects

httpbin.org has a redirect chain:

```bash
curl -v -L https://httpbin.org/redirect/5
```

How many redirects did it follow before reaching the final destination?

## Challenge 4 — Upload a file

Create a file and upload it:

```bash
echo "hello, curl" > /tmp/upload.txt
curl -v -F "file=@/tmp/upload.txt" https://httpbin.org/post
```

## Challenge 5 — Custom everything

Send a request with:
- Method: PUT
- Body: `{"status":"testing"}`
- Header: `X-Trace: day07-drill`
- Content-Type: application/json
- Timeout: 5 seconds

Write the curl command yourself.

## Challenge 6 — Script it

Write a bash one-liner that:
1. Fetches `https://api.github.com/users/scott`
2. Only prints the status code
3. Only prints the `Content-Type` header

## Deliverables

1. The command from Challenge 5
2. The one-liner from Challenge 6
3. How many redirects in Challenge 3?

## Hints

<details>
<summary>Click for hints</summary>

- Challenge 5: `-X PUT -d '{}' -H "Content-Type: application/json" -H "X-Trace: day07-drill" --max-time 5`
- Challenge 6: `curl -s -o /dev/null -w "%{http_code}\n" URL` and `curl -s -I URL | grep -i content-type`
- `-L` is needed to follow redirects automatically
</details>
