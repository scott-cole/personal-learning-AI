# DAY19: DNS — How the Internet Finds Servers

---

## The anecdote: "DNS is a phonebook for the internet"

Before smartphones, you had a phonebook:

| Phonebook | DNS |
|-----------|-----|
| You look up "Scott's Pizza" | Browser looks up `example.com` |
| Phonebook returns "(555) 123-4567" | DNS returns `93.184.216.34` (IP address) |
| You dial the number | Browser connects to the IP |
| If Scott's Pizza moves, they update the phonebook | DNS records are updated when servers move |

You can't dial a phone number from a name. You can't connect to a server from `example.com`. DNS converts human-readable names to machine-readable IP addresses.

---

## How DNS resolution works

```
Your browser                            DNS Resolver (ISP/Cloudflare/Google)
     |                                            |
     | "What is the IP for example.com?"          |
     |------------------------------------------->|
     |                                            |  1. Check cache
     |                                            |  2. If not cached, ask root server
     |                                            |  3. Ask .com TLD server
     |                                            |  4. Ask example.com's authoritative server
     |  "93.184.216.34"                           |
     |<-------------------------------------------|
     |                                            |
     | Connect to 93.184.216.34:443               |
     |------------------------------------------->|
     |                                            |
     | HTTPS Response                             |
     |<-------------------------------------------|
```

---

## DNS record types

| Record | What it does | Example |
|--------|-------------|---------|
| **A** | Maps hostname → IPv4 address | `example.com → 93.184.216.34` |
| **AAAA** | Maps hostname → IPv6 address | `example.com → 2606:2800:220:1:248:1893:25c8:1946` |
| **CNAME** | Maps hostname → another hostname (alias) | `www.example.com → example.com` |
| **MX** | Mail server address | `@ → mail.example.com` |
| **TXT** | Arbitrary text (verification, SPF) | `v=spf1 include:_spf.google.com ~all` |
| **NS** | Name server for the domain | `example.com → ns1.example.com` |

```bash
# Look up A record (IPv4)
dig example.com A +short
# Or: host example.com

# Look up CNAME
dig www.github.com CNAME +short

# Look up MX (mail)
dig gmail.com MX +short

# Look up TXT records
dig google.com TXT +short
```

---

## TTL — Time To Live

Every DNS record has a TTL (Time To Live) — how long the resolver should cache the result:

```bash
# Check TTL
dig example.com
# Look for: "example.com. 3600 IN A 93.184.216.34"
# The 3600 is the TTL in seconds (1 hour)
```

| TTL | Duration | Use case |
|-----|----------|----------|
| 300 | 5 minutes | Emergency changes, load balancing |
| 3600 | 1 hour | Default for most records |
| 86400 | 24 hours | Stable records |
| 604800 | 7 days | Very stable (MX records) |

**Low TTL = faster updates, more DNS queries.** **High TTL = faster response, cached longer.**

---

## DNS propagation delay

When you change a DNS record, it takes time to reach everyone:

1. You update `example.com` A record to new IP
2. Your DNS provider updates their authoritative server
3. Resolvers that had the old IP cached will keep using it until TTL expires
4. After TTL, they check again and get the new IP

This is called **propagation delay**. Always lower the TTL *before* making a change, then raise it back after.

---

## Using curl with DNS

```bash
# Resolve a hostname manually (override DNS)
curl --resolve "example.com:443:93.184.216.34" https://example.com

# Use a specific DNS server
curl --dns-servers 8.8.8.8 https://example.com

# See which IP curl resolved to
curl -v https://example.com 2>&1 | grep "Trying"
```

---

## Senior mindset: "DNS is the first thing to check"

When something doesn't work:

1. `ping example.com` — does it resolve?
2. `dig example.com` — what records exist?
3. `curl -v https://example.com` — what IP is it trying?

80% of "the site is down" problems are DNS problems.

---

## Summary

| Concept | Tool | What you see |
|---------|------|-------------|
| A record | `dig example.com A +short` | IPv4 address |
| CNAME | `dig www.example.com CNAME +short` | Canonical name |
| MX record | `dig example.com MX +short` | Mail server |
| TTL | `dig example.com` | Cache duration in seconds |
| Propagation | Wait | Time for DNS changes to spread |
