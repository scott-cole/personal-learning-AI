# DAY11: HTTP Handlers — "The Receptionist"

---

## The anecdote: "A handler is a receptionist"

A customer walks in. The receptionist:
1. Greets them (parse request)
2. Figures out what they want (route)
3. Calls the right person (service)
4. Gives the response (write response)

```go
func Handler(w http.ResponseWriter, r *http.Request) {
    // w = where to write the response
    // r = what the client sent
}
```

---

## Go's HTTP handler

```go
func helloHandler(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Hello, world!")
}
```

That's it. That's a handler.

**`http.ResponseWriter`** — where you write the response
**`*http.Request`** — contains method, URL, headers, body

---

## Routing with `http.ServeMux`

```go
mux := http.NewServeMux()
mux.HandleFunc("GET /", helloHandler)     // Go 1.22+ pattern matching
mux.HandleFunc("POST /shorten", shortenHandler)

http.ListenAndServe(":8080", mux)
```

Go 1.22 added method-based routing: `"GET /path"`, `"POST /path"`.

---

## Reading input

```go
// Query parameter
code := r.URL.Query().Get("code")

// Path value (Go 1.22+)
code := r.PathValue("code")  // for routes like "GET /{code}"

// Request body
body, err := io.ReadAll(r.Body)
```

## Writing output

```go
// Plain text
fmt.Fprintf(w, "Hello")

// Status code
w.WriteHeader(http.StatusNotFound)  // 404

// JSON (manually for now)
w.Header().Set("Content-Type", "application/json")
fmt.Fprintf(w, `{"key": "value"}`)
```

---

## Senior mindset: "Parse, validate, respond"

Every handler follows the same pattern:

```go
func handleSomething(w http.ResponseWriter, r *http.Request) {
    // 1. Parse input (query, body, path)
    // 2. Validate input
    // 3. Call service
    // 4. Write response or error
    if err != nil {
        http.Error(w, err.Error(), http.StatusBadRequest)
        return
    }
}
```

Open DAY11/DRILL.md.
