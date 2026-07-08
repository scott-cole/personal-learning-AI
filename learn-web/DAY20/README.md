# DAY20: TLS/SSL — Handshake, Certificates, and HTTPS

---

## The anecdote: "TLS is a tamper-proof envelope"

You need to send a secret letter across town:

| Without TLS | With TLS |
|-------------|----------|
| Write on a postcard | Put letter in a tamper-proof envelope |
| Anyone can read it | Encrypted — only recipient can read |
| Anyone can rewrite it | Tamper-evident seal |
| No way to verify it's really from you | Signed by a trusted authority |

HTTPS = HTTP + TLS. The "S" stands for **Secure**.

---

## Why HTTP is not enough

```bash
# This is a plaintext conversation
curl -v http://example.com
# Anyone on your WiFi network can see:
# - The URL you're visiting
# - The data you send and receive
# - Your cookies (and steal your session)
```

HTTPS encrypts everything:

```bash
# This is encrypted
curl -v https://example.com
# Observers only see:
# - That you connected to example.com
# - The IP address and port
# - Nothing else
```

---

## The TLS handshake — what happens before your first byte

```
Client                          Server
  |                               |
  |--- ClientHello -------------->|  "I support TLS 1.3, these ciphers..."
  |<--- ServerHello ------------- |  "Let's use TLS 1.3, this cipher"
  |<--- Certificate -------------|  "Here's my certificate (signed by CA)"
  |<--- ServerHelloDone ---------|  "I'm done"
  |                               |
  |--- ClientKeyExchange -------->|  "Here's the shared secret (encrypted)"
  |--- ChangeCipherSpec --------->|  "Everything after this is encrypted"
  |--- Finished ----------------->|  "Let's verify"
  |                               |
  |<--- ChangeCipherSpec ---------|  "Everything after this is encrypted"
  |<--- Finished -----------------|  "Verified!"
  |                               |
  |====== Encrypted conversation =|
  |=== HTTP requests/responses ===|
```

~2 round trips before any HTTP data is sent. With TLS 1.3, it's 1 round trip (or 0 for repeat connections).

---

## Certificates — who signed this?

A TLS certificate contains:

- Domain name (CN/SAN)
- Public key
- Issuer (who signed it)
- Valid from / Valid to dates
- Fingerprint (hash)

```bash
# View a server's certificate
openssl s_client -connect example.com:443 -servername example.com 2>&1 </dev/null

# Simplified — just the certificate chain
echo | openssl s_client -connect google.com:443 -servername google.com 2>&1 | \
  openssl x509 -noout -subject -issuer -dates
```

---

## Certificate Authorities (CAs)

A certificate is trusted because a **Certificate Authority** signed it. Your OS/browser has a built-in list of trusted CAs.

```
Your Browser                  example.com
    |                             |
    |  "I trust DigiCert"          |
    |  "DigiCert signed this"      |
    |  "Therefore I trust this"    |
    |                             |
    |  ✓ Certificate valid        |
    |  ✓ Domain matches           |
    |  ✓ Not expired              |
    |  ✓ Chain of trust intact    |
    |                             |
```

---

## Self-signed certificates (for development)

```bash
# Generate a self-signed cert (DO NOT use in production)
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem \
  -days 365 -nodes -subj "/CN=localhost"

# Use it with a Go server (and skip verification in curl)
curl -k https://localhost:8080  # -k skips certificate verification
```

---

## HTTPS in Go

```go
package main

import (
    "fmt"
    "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "This connection is: %s\n", r.TLS.Version)
}

func main() {
    http.HandleFunc("/", handler)
    fmt.Println("Server on :443 (HTTPS)")
    
    // In production, use certs from Let's Encrypt
    // For local dev: generate self-signed certs
    err := http.ListenAndServeTLS(":443", "cert.pem", "key.pem", nil)
    if err != nil {
        fmt.Println("Error:", err)
    }
}
```

---

## Senior mindset: "Always HTTPS, always redirect"

```nginx
# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com;
    return 301 https://$server_name$request_uri;
}
```

**Every site should use HTTPS.** There's no excuse in 2026 — Let's Encrypt provides free certs, and services like Cloudflare offer free TLS termination.

---

## Summary

| Concept | What it does | Tool |
|---------|-------------|------|
| TLS | Encrypts HTTP traffic | Built into HTTPS |
| Certificate | Proves server identity | `openssl s_client` |
| CA | Trusted third-party signer | Let's Encrypt, DigiCert |
| Handshake | Establish encrypted session | ~1-2 round trips |
| Port 443 | Default HTTPS port | `https://...:443` |
| Self-signed | Dev-only certs | `openssl req -x509` |
