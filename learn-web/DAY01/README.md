# DAY01: What is HTTP? The Request/Response Model

---

## The anecdote: "HTTP is a postal service"

Imagine you want to buy a book from a shop across town. You:

1. **Write a letter** — "Please send me *The Go Programming Language*"
2. **Put it in an envelope** with the shop's address and your return address
3. **Post it** — the postal service delivers it
4. **The shop reads your letter**, finds the book, puts it in a box
5. **They mail the box back** with your return address

HTTP works exactly the same way.

```
You (client)                      Server
   |                                |
   |--- GET /books/42 ------------>|
   |                                |  Looks up book #42
   |<--- 200 OK + book data -------|
   |                                |
```

- **You** are the **client** (curl, browser, your Go program)
- **The shop** is the **server** (a computer somewhere on the internet)
- **The letter** is the **request**
- **The box they send back** is the **response**

Every time you visit a website, your browser sends an HTTP request to a server. The server sends back an HTTP response. The web browser renders that response as a page.

---

## What's in a URL?

Every request needs an address. On the web, that's a **URL** (Uniform Resource Locator).

```
  https://api.example.com:443/products?page=2#reviews
  \___/  \_____________/ \__/ \________________/\_____/
   |           |          |          |            |
 scheme      host       port       query       fragment
```

| Part | Example | What it does |
|------|---------|--------------|
| Scheme | `https` | The protocol (HTTP or HTTPS) |
| Host | `api.example.com` | The server's address (like a street address) |
| Port | `:443` | Which door to knock on (80 for HTTP, 443 for HTTPS) |
| Path | `/products` | Which shelf in the shop |
| Query | `?page=2` | Extra instructions (search, pagination) |
| Fragment | `#reviews` | A specific spot on the page (never sent to the server) |

Try it yourself — paste any URL into your browser's address bar. That's an HTTP request happening in real time.

---

## The anatomy of a request

Every HTTP request has:

```
GET /products HTTP/1.1
Host: api.example.com
User-Agent: curl/8.0
Accept: */*
```

1. **Request line** — method (`GET`), path (`/products`), HTTP version (`HTTP/1.1`)
2. **Headers** — key-value pairs with metadata (`Host`, `User-Agent`, `Accept`)
3. **Body** — optional data (used with POST, PUT, PATCH)

---

## The anatomy of a response

Every HTTP response has:

```
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 42

{"product":"book","price":29.99}
```

1. **Status line** — version (`HTTP/1.1`), status code (`200`), reason (`OK`)
2. **Headers** — metadata about the response
3. **Body** — the actual data (HTML, JSON, image, etc.)

---

## See it for yourself

Open your terminal and run:

```bash
curl -v https://example.com
```

The `-v` flag shows *everything* — the request being sent, the headers, and the response. This is the single most useful command for learning HTTP.

You'll see output like:

```
* Request:
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/8.0
> Accept: */*
>
* Response:
< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 1256
<
<!doctype html>...
```

---

## Senior mindset: "Read the protocol, not the library"

Junior engineers learn a library (like `axios` or `requests`). Senior engineers understand the protocol underneath.

Every HTTP library in every language is just a wrapper around the same thing: a TCP connection that sends text formatted as an HTTP request and reads back text formatted as an HTTP response.

That's it. There's no magic. Just formatted text.

---

## Summary

| Concept | Description |
|---------|-------------|
| HTTP | A protocol for sending formatted text between clients and servers |
| Request | What the client sends (method, path, headers, body) |
| Response | What the server sends back (status, headers, body) |
| URL | The address of a resource on the web |
| `curl -v` | See the raw HTTP conversation |
