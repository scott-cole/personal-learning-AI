# DAY07: cURL Deep Dive — All the Flags You Need

---

## The anecdote: "cURL is a telephone for talking to servers"

A telephone lets you call anyone and have a conversation. You dial a number, speak, listen, and hang up.

cURL is the same — it's a tool for "calling" any server that speaks HTTP (or FTP, or SMTP, or 20+ other protocols) and having a conversation.

The name "cURL" stands for "Client URL." It's been around since 1997 and is installed on basically every Unix system (and Windows 10+).

---

## Essential flags (you'll use these daily)

```bash
# Make a request and see everything
curl -v https://api.example.com

# Just the response body (the default)
curl https://api.example.com

# Follow redirects
curl -L http://example.com

# Save output to a file
curl -o output.html https://example.com

# Save with the remote filename
curl -O https://example.com/file.zip

# Silent mode (no progress, no errors)
curl -s https://api.example.com

# Silent but show errors (-sS)
curl -sS https://api.example.com
```

---

## Request control

```bash
# HTTP method
curl -X POST https://api.example.com
curl -X PUT https://api.example.com
curl -X DELETE https://api.example.com

# Send body data
curl -d '{"key":"value"}' https://api.example.com

# Send body from file
curl -d @data.json https://api.example.com

# URL-encoded form
curl -d "name=Scott&age=30" https://api.example.com

# Multipart (file upload)
curl -F "image=@photo.jpg" https://api.example.com/upload

# Custom header
curl -H "Authorization: Bearer TOKEN" https://api.example.com

# Multiple headers
curl -H "Accept: application/json" -H "X-Debug: true" https://api.example.com

# User-Agent override
curl -A "MyBot/1.0" https://api.example.com

# Referer
curl -e "https://google.com" https://api.example.com
```

---

## Output control

```bash
# Write to file
curl -o result.json https://api.example.com

# Write-output format (status code, size, timing)
curl -w "Status: %{http_code}\nTime: %{time_total}s\n" -s -o /dev/null https://example.com

# All timing metrics
curl -w "@timing-format.txt" -s -o /dev/null https://example.com
```

Where `timing-format.txt` contains:

```
    time_namelookup:  %{time_namelookup}s\n
       time_connect:  %{time_connect}s\n
    time_appconnect:  %{time_appconnect}s\n
   time_pretransfer:  %{time_pretransfer}s\n
      time_redirect:  %{time_redirect}s\n
 time_starttransfer:  %{time_starttransfer}s\n
                    ----------\n
         time_total:  %{time_total}s\n
```

---

## Authentication

```bash
# Basic auth
curl -u username:password https://api.example.com

# Bearer token
curl -H "Authorization: Bearer TOKEN" https://api.example.com

# API key as header
curl -H "X-API-Key: abc123" https://api.example.com
```

---

## Debugging

```bash
# See EVERYTHING (including TLS handshake)
curl -v https://example.com

# Even more verbose (TLS details)
curl --trace-ascii - https://example.com

# Just the response headers
curl -I https://example.com

# Just the status code (for scripts)
curl -s -o /dev/null -w "%{http_code}" https://example.com
```

---

## Advanced

```bash
# Limit rate (don't overwhelm the server)
curl --limit-rate 200K https://example.com/large-file.zip

# Max time (timeout)
curl --max-time 10 https://api.example.com

# Retry on failure
curl --retry 3 --retry-delay 5 https://api.example.com

# Cookie jar (store cookies)
curl -c cookies.txt -b cookies.txt https://example.com

# Follow redirects (with limit)
curl -L --max-redirs 5 https://example.com

# Skip TLS verification (DANGER: only for testing)
curl -k https://self-signed.example.com
```

---

## Senior mindset: "cURL is your swiss army knife"

Every senior engineer has curl burned into their muscle memory. When debugging:
1. Reproduce the issue with curl first (remove all the library complexity)
2. Once the raw curl command works, translate it back to your language's HTTP library

This isolates the problem: is it your code, or the network, or the server?

---

## Summary

| Category | Key flags |
|----------|-----------|
| Verbosity | `-v`, `-I`, `--trace-ascii` |
| Method | `-X GET/POST/PUT/PATCH/DELETE` |
| Data | `-d`, `-F`, `-H` |
| Output | `-o`, `-O`, `-w`, `-s` |
| Auth | `-u`, `-H "Authorization: ..."` |
| Safety | `--max-time`, `--retry`, `--limit-rate` |
