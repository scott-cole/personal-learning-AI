# DAY20: Drill — "Inspect the TLS handshake"

Use `openssl` and `curl` to inspect TLS certificates and handshakes.

## Part 1 — View a certificate

```bash
# View the full certificate chain
echo | openssl s_client -connect example.com:443 -servername example.com 2>&1 | less

# View just the certificate details
echo | openssl s_client -connect example.com:443 -servername example.com 2>&1 | \
  openssl x509 -noout -text | less
```

Look for:
- Subject (who the cert is for)
- Issuer (who signed it)
- Validity dates
- Subject Alternative Names (SANs)

## Part 2 — Compare HTTP vs HTTPS

```bash
# With curl -v, compare:
curl -v http://example.com 2>&1 | head -20
curl -v https://example.com 2>&1 | head -20

# Notice:
# - Which one has TLS details?
# - Which one uses port 80 vs 443?
# - What does curl print during the TLS handshake?
```

## Part 3 — Check certificate expiry

```bash
# Get expiry date
echo | openssl s_client -connect github.com:443 -servername github.com 2>&1 | \
  openssl x509 -noout -dates

# Is it still valid? When does it expire?
```

## Part 4 — Self-signed certificates

Create a self-signed cert and a Go HTTPS server:

```bash
# Generate cert and key
openssl req -x509 -newkey rsa:2048 -keyout /tmp/key.pem -out /tmp/cert.pem \
  -days 365 -nodes -subj "/CN=localhost"
```

Create `server.go`:

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "HTTPS works!\n")
    if r.TLS != nil {
        fmt.Fprintf(w, "TLS version: %s\n", r.TLS.Version)
    }
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("Server on :8443 (HTTPS)")
    http.ListenAndServeTLS(":8443", "/tmp/cert.pem", "/tmp/key.pem", nil)
}
```

```bash
go run server.go &

# Without -k (should fail)
curl -v https://localhost:8443 2>&1 | grep -E "error|certificate"

# With -k (skip verification)
curl -v -k https://localhost:8443 2>&1 | grep -E "HTTPS works|TLS version"

kill %1
```

## Part 5 — Check a site's TLS version

```bash
# Find which TLS version a site supports
echo | openssl s_client -connect google.com:443 -servername google.com \
  -tls1_2 2>&1 | grep "Protocol"
echo | openssl s_client -connect google.com:443 -servername google.com \
  -tls1_3 2>&1 | grep "Protocol"
```

## Deliverables

1. Who issued the certificate for `example.com`?
2. When does the `github.com` certificate expire?
3. What error did you get without `-k` in Part 4?
4. What TLS version does `google.com` use?

## Hints

<details>
<summary>Click for hints</summary>

- `openssl s_client` is interactive — the `echo |` pipes nothing to it so it exits immediately
- `-servername` is required for TLS SNI (multiple sites on one IP)
- Self-signed certs are not trusted by default — use `-k` to skip verification
- TLS 1.3 is the current standard, TLS 1.2 is the fallback
- Let's Encrypt issues free certs valid for 90 days
</details>
