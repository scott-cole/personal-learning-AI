# DAY21: Drill — "Talk over a WebSocket"

Build a WebSocket echo server and client, then inspect the handshake.

## Part 1 — Inspect the WebSocket handshake

```bash
# First, let's see the upgrade headers
curl -v -H "Connection: Upgrade" -H "Upgrade: websocket" \
  -H "Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==" \
  -H "Sec-WebSocket-Version: 13" \
  https://echo.websocket.org/echo 2>&1
```

What status code do you get? What does the `Sec-WebSocket-Accept` header look like?

## Part 2 — Install gorilla/websocket

```bash
go get github.com/gorilla/websocket
```

## Part 3 — Build an echo server

Create `server.go`:

```go
package main

import (
    "fmt"
    "log"
    "net/http"
    "github.com/gorilla/websocket"
)

var upgrader = websocket.Upgrader{
    CheckOrigin: func(r *http.Request) bool { return true },
}

func echoHandler(w http.ResponseWriter, r *http.Request) {
    conn, err := upgrader.Upgrade(w, r, nil)
    if err != nil {
        log.Print("Upgrade failed:", err)
        return
    }
    defer conn.Close()
    log.Printf("Client connected from %s", conn.RemoteAddr())

    for {
        messageType, message, err := conn.ReadMessage()
        if err != nil {
            log.Println("Read error:", err)
            break
        }
        log.Printf("Received: %s", message)
        err = conn.WriteMessage(messageType, message)
        if err != nil {
            log.Println("Write error:", err)
            break
        }
    }
}

func main() {
    http.HandleFunc("/echo", echoHandler)
    fmt.Println("WebSocket server on ws://localhost:8080/echo")
    log.Fatal(http.ListenAndServe(":8080", nil))
}
```

## Part 4 — Build a client

Create `client.go`:

```go
package main

import (
    "bufio"
    "fmt"
    "log"
    "os"
    "github.com/gorilla/websocket"
)

func main() {
    url := "ws://localhost:8080/echo"
    conn, _, err := websocket.DefaultDialer.Dial(url, nil)
    if err != nil {
        log.Fatal("Dial failed:", err)
    }
    defer conn.Close()

    go func() {
        for {
            _, message, err := conn.ReadMessage()
            if err != nil {
                return
            }
            fmt.Printf("Server: %s\n", message)
        }
    }()

    scanner := bufio.NewScanner(os.Stdin)
    for scanner.Scan() {
        text := scanner.Text()
        if text == "quit" {
            break
        }
        conn.WriteMessage(websocket.TextMessage, []byte(text))
    }
}
```

## Part 5 — Run it

```bash
# Terminal 1: start the server
go run server.go

# Terminal 2: start the client
go run client.go

# Type messages in the client — they should echo back
hello
Server: hello
how are you?
Server: how are you?
quit
```

## Part 6 — Watch the handshake with curl (bonus)

```bash
# While the server is running:
curl -v -H "Connection: Upgrade" -H "Upgrade: websocket" \
  -H "Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==" \
  -H "Sec-WebSocket-Version: 13" \
  http://localhost:8080/echo
# Look for: 101 Switching Protocols
```

## Deliverables

1. The handshake response from Part 1 (status code + headers)
2. Your server and client running successfully
3. What happens when you disconnect the client?

## Hints

<details>
<summary>Click for hints</summary>

- `ws://` is the WebSocket URL scheme (like `http://`). `wss://` is secure (like `https://`)
- The gorilla/websocket library handles the handshake automatically
- WebSocket connections stay open until explicitly closed
- `conn.ReadMessage()` blocks until a message arrives — that's why we use a goroutine
- The echo server at `echo.websocket.org` is a public test server (may be unreliable)
</details>
