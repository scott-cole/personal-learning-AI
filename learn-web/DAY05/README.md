# DAY05: HTTP Request & Response Body

---

## The anecdote: "The body is the package inside the envelope"

Headers are the envelope — address, stamp, handling instructions. But the **body** is what you actually care about: the gift, the documents, the merchandise.

In HTTP:
- **Headers** describe the body (it's JSON, it's 42 bytes, it's compressed)
- **Body** is the actual data

---

## What goes in a body?

| Content Type | What it looks like | Where it's used |
|---|---|---|
| `application/json` | `{"name":"Scott","age":30}` | Most modern APIs |
| `application/x-www-form-urlencoded` | `name=Scott&age=30` | HTML forms |
| `multipart/form-data` | Binary boundary-separated parts | File uploads |
| `text/plain` | `Hello, world!` | Simple text |
| `text/html` | `<html>...</html>` | Web pages |
| `image/png` | Binary bytes | Images |
| `application/octet-stream` | Raw binary | File downloads |

---

## Sending a JSON body

```bash
curl -X POST https://jsonplaceholder.typicode.com/posts \
  -H "Content-Type: application/json" \
  -d '{"title":"My Post","body":"Hello!","userId":1}'
```

The `-d` flag sends a body. By default, `-d` sends with `Content-Type: application/x-www-form-urlencoded`. Adding `-H "Content-Type: application/json"` overrides this.

**What JSON looks like in Go:**

```go
import (
    "bytes"
    "encoding/json"
    "net/http"
)

type Post struct {
    Title  string `json:"title"`
    Body   string `json:"body"`
    UserID int    `json:"userId"`
}

func sendPost() {
    post := Post{Title: "My Post", Body: "Hello!", UserID: 1}
    jsonData, _ := json.Marshal(post)

    resp, _ := http.Post(
        "https://jsonplaceholder.typicode.com/posts",
        "application/json",
        bytes.NewBuffer(jsonData),
    )
    defer resp.Body.Close()
}
```

---

## Reading a JSON body

```go
import (
    "encoding/json"
    "io"
    "net/http"
)

func getPost(id int) (*Post, error) {
    resp, err := http.Get(fmt.Sprintf(
        "https://jsonplaceholder.typicode.com/posts/%d", id,
    ))
    if err != nil {
        return nil, err
    }
    defer resp.Body.Close()

    body, _ := io.ReadAll(resp.Body)

    var post Post
    json.Unmarshal(body, &post)
    return &post, nil
}
```

Always `defer resp.Body.Close()` — leaking response bodies is a common Go mistake.

---

## Form data (URL-encoded)

HTML forms send data URL-encoded by default:

```bash
curl -X POST https://httpbin.org/post \
  -d "name=Scott&age=30&city=London"
```

What curl sends on the wire:

```
POST /post HTTP/1.1
Host: httpbin.org
Content-Type: application/x-www-form-urlencoded
Content-Length: 28

name=Scott&age=30&city=London
```

---

## Multipart form data (file uploads)

```bash
curl -X POST https://httpbin.org/post \
  -F "name=Scott" \
  -F "file=@/path/to/file.txt"
```

The `-F` flag switches to `multipart/form-data`. Each part is separated by a boundary string in the body.

---

## The request body lifecycle

```
Client                                Server
  |                                     |
  |--- Request headers --------------->|
  |--- Request body (JSON) ----------->|  Server reads body
  |                                     |  Server processes
  |<--- Response headers --------------|
  |<--- Response body (JSON) ----------|  Client reads body
  |                                     |
```

---

## Senior mindset: "Stream, don't load"

A junior reads the entire body into memory. A senior streams it.

```go
// Junior — loads entire body into memory
body, _ := io.ReadAll(resp.Body)

// Senior — streams through the JSON decoder
decoder := json.NewDecoder(resp.Body)
var post Post
decoder.Decode(&post)
```

For JSON, `json.NewDecoder` streams. For large files, use `io.Copy` to disk rather than loading into RAM.

---

## Summary

| Format | curl flag | Content-Type | Use case |
|--------|-----------|--------------|----------|
| JSON | `-d '{}'` + `-H "Content-Type: application/json"` | `application/json` | APIs |
| Form | `-d "key=val"` | `application/x-www-form-urlencoded` | HTML forms |
| Multipart | `-F "key=val"` | `multipart/form-data` | File uploads |
| Raw | `-d @file.txt` | `text/plain` | Custom data |
