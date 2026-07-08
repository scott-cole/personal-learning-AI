# DAY19: Drill — "Become a DNS detective"

Use `dig`, `host`, and `curl` to investigate DNS records.

## Part 1 — Basic lookups

```bash
# A record (IPv4)
dig example.com A +short

# AAAA record (IPv6)
dig example.com AAAA +short

# All records
dig example.com ANY +short
```

## Part 2 — Trace a CNAME chain

```bash
# Follow the CNAME chain
dig www.github.com CNAME +short
dig github.com A +short
```

Some sites use multiple CNAMEs through a CDN. How many hops does `www.github.com` take?

## Part 3 — Check TTLs

```bash
dig example.com

# Look for the TTL value (the number before "IN A")
# Is it 300 (5 min), 3600 (1 hour), or 86400 (24 hours)?
```

## Part 4 — Use different DNS resolvers

```bash
# Default resolver (your ISP)
dig example.com

# Google DNS
dig @8.8.8.8 example.com

# Cloudflare DNS
dig @1.1.1.1 example.com

# Do the results differ?
```

## Part 5 — DNS in Go

Create `main.go`:

```go
package main

import (
    "fmt"
    "net"
    "os"
)

func main() {
    if len(os.Args) < 2 {
        fmt.Println("Usage: go run main.go <hostname>")
        os.Exit(1)
    }
    hostname := os.Args[1]

    // Lookup IP addresses
    ips, err := net.LookupHost(hostname)
    if err != nil {
        fmt.Println("Error:", err)
        os.Exit(1)
    }
    fmt.Printf("IPs for %s:\n", hostname)
    for _, ip := range ips {
        fmt.Printf("  %s\n", ip)
    }

    // Lookup CNAME
    cname, err := net.LookupCNAME(hostname)
    if err == nil {
        fmt.Printf("CNAME: %s\n", cname)
    }

    // Lookup MX records
    mxs, err := net.LookupMX(hostname)
    if err == nil {
        fmt.Println("Mail servers:")
        for _, mx := range mxs {
            fmt.Printf("  %s (priority %d)\n", mx.Host, mx.Pref)
        }
    }
}
```

Run:

```bash
go run main.go example.com
go run main.go google.com
go run main.go github.com
```

## Part 6 — Propagation simulation

```bash
# Override DNS with --resolve (simulate a DNS change)
curl --resolve "example.com:443:127.0.0.1" https://example.com
# This should fail — we told curl to connect to localhost
```

## Deliverables

1. IP addresses for `example.com`, `google.com`, `github.com`
2. The CNAME chain for `www.github.com`
3. TTL for `example.com`
4. Output from your Go program

## Hints

<details>
<summary>Click for hints</summary>

- `dig` is probably preinstalled on macOS. If not: `brew install bind`
- `@8.8.8.8` uses Google's DNS resolver directly (bypasses your ISP)
- CNAMEs are aliases — `www.github.com` points to something else which has the actual A record
- `net.LookupHost` returns all IP addresses (both A and AAAA)
- `--resolve` is useful for testing a site before DNS is updated
</details>
