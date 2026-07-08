# DAY21: WebSockets — Always-On Connections

---

## The anecdote: "WebSockets are a walkie-talkie — always-on connection"

HTTP is like a walkie-talkie where you must say "over" after every message:

```
You: "What's the weather?" (over)
Friend: "Sunny" (over)
You: "What about tomorrow?" (over)
Friend: "Also sunny" (over)
```

A WebSocket is a real walkie-talkie where you just leave it on:

```
You: "What's the weather?"
Friend: "Sunny"
You: "What about tomorrow?"
Friend: "Also sunny"
Friend: *5 minutes later* "Actually, it's raining now"
```

No overhead. No repeated headers. Either side can send data at any time.

---

## HTTP vs WebSocket

| Feature | HTTP | WebSocket |
|---------|------|-----------|
| Connection | Short-lived (one request = one response) | Long-lived (persistent) |
| Direction | Client → Server (request) | Bidirectional |
| Overhead | Headers on every request | Minimal framing |
| Latency | Request-response cycle | Real-time |
| Use case | REST APIs, web pages | Chat, games, live data |

---

## The WebSocket handshake

A WebSocket starts as an HTTP request, then **upgrades**:

```text
Client → Server:
GET /chat HTTP/1.1
Host: example.com
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13

Server → Client:
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=
```

Status code **101 Switching Protocols** means "OK, we're switching to WebSocket."

```
Client                         Server
  |                              |
  |--- HTTP Upgrade request ---->|
  |<--- 101 Switching Protocols  |
  |                              |
  |======= WebSocket frames =====|
  |  Bidirectional, low-latency  |
  |  No HTTP headers             |
  |  Any time, either side       |
  |==============================|
```

---

## WebSocket frames

After the handshake, data is sent in **frames** (not HTTP requests):

| Frame type | Purpose |
|------------|---------|
| Text | UTF-8 text (most common) |
| Binary | Binary data (images, protobuf) |
| Ping/Pong | Keep-alive (connection health check) |
| Close | End the connection |

---

## WebSocket in Go (client)

```go
package main

import (
    "fmt"
    "net/url"
    "github.com/gorilla/websocket"
)

func main() {
    u := url.URL{Scheme: "wss", Host: "echo.websocket.org", Path: "/echo"}
    conn, _, err := websocket.DefaultDialer.Dial(u.String(), nil)
    if err != nil {
        fmt.Println("Error:", err)
        return
    }
    defer conn.Close()

    // Send a message
    conn.WriteMessage(websocket.TextMessage, []byte("Hello, WebSocket!"))

    // Read the response
    _, msg, err := conn.ReadMessage()
    if err != nil {
        fmt.Println("Error reading:", err)
        return
    }
    fmt.Printf("Received: %s\n", msg)
}
```

---

## WebSocket in Go (server)

```go
package main

import (
    "fmt"
    "net/http"
    "github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
    CheckOrigin: func(r *http.Request) bool { return true },
}

func handler(w http.ResponseWriter, r *http.Request) {
    conn, err := upgrader.Upgrade(w, r, nil)
    if err != nil {
        fmt.Println("Upgrade error:", err)
        return
    }
    defer conn.Close()

    for {
        // Read message from client
        msgType, msg, err := conn.ReadMessage()
        if err != nil {
            break
        }
        // Echo it back
        conn.WriteMessage(msgType, msg)
    }
}

func main() {
    http.HandleFunc("/echo", handler)
    http.ListenAndServe(":8080", nil)
}
```

---

## When to use WebSocket vs HTTP

| Use WebSocket when | Use HTTP when |
|-------------------|---------------|
| Real-time updates (chat, notifications) | Request-response data |
| Live data streams (stock tickers) | CRUD operations |
| Multiplayer games | File uploads/downloads |
| Collaborative editing | REST API calls |
| Low-latency needed | Can tolerate polling |

---

## Senior mindset: "WebSocket is not a replacement for HTTP"

WebSocket solves a specific problem: **bidirectional, low-latency communication**. It doesn't replace HTTP for normal API calls.

Many apps use both: HTTP for REST API calls, WebSocket for real-time events.

---

## Summary

| Concept | HTTP | WebSocket |
|---------|------|-----------|
| Connection | Request-response | Persistent |
| Initiation | Client only | Client (via HTTP upgrade) |
| Messages | Headers + body every time | Framed data |
| Direction | Client → Server | Bidirectional |
| Status code | 200, 404, etc. | 101 Switching Protocols |
| Port | 80/443 | Same (starts as HTTP) |
