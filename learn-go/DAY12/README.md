# DAY12: JSON — "The Common Language"

---

## The anecdote: "JSON is the common language"

You speak English. I speak English. We can communicate.

Your Go program speaks structs. A JavaScript frontend speaks objects. **JSON** is the common language they both understand.

```
Go struct   →  marshal →  JSON string
JSON string →  unmarshal →  Go struct
```

---

## Marshal (Go → JSON)

```go
type Person struct {
    Name string `json:"name"`
    Age  int    `json:"age"`
}

p := Person{Name: "Scott", Age: 30}
jsonBytes, err := json.Marshal(p)
// jsonBytes = `{"name":"Scott","age":30}`
```

**Tags** (`json:"name"`) tell Go what field name to use in JSON. Without them, the field name is used verbatim (e.g., `Name`, not `name`).

### Marshal vs NewEncoder

Both convert Go → JSON. The difference is **where the output goes**:

```go
// json.Marshal — returns []byte, good for files/strings
jsonBytes, _ := json.Marshal(p)
os.WriteFile("person.json", jsonBytes, 0644)

// json.NewEncoder — writes directly to a writer, good for HTTP
json.NewEncoder(w).Encode(p)
```

| Use `json.Marshal` | Use `json.NewEncoder` |
|---|---|
| Writing to a file | Writing to HTTP response |
| Need the bytes as a variable (e.g., to hash) | Streaming large output |
| One-off conversion | Every HTTP response |

---

## JSON tags: omitempty, string, and "-"

```go
type User struct {
    ID      int    `json:"id"`
    Name    string `json:"name"`
    Email   string `json:"email,omitempty"` // skip if empty
    Token   string `json:"-"`               // never include in JSON
    Score   int    `json:"score,string"`    // encode as string "42" not 42
}
```

- **`omitempty`** — if value is zero (empty string, 0, false, nil), **omit the field entirely** from the JSON output
  ```go
  u := User{ID: 1, Name: "Scott", Email: "", Token: "secret"}
  json.Marshal(u)
  // {"id":1,"name":"Scott"}  ← no "email" field (empty), no "token" (-)
  ```
- **`"-"`** — never include this field in JSON (useful for passwords, secrets, internal state)
- **`"fieldname,string"`** — encode the field as a JSON string instead of its native type

---

## Unmarshal (JSON → Go)

```go
input := `{"name":"Scott","age":30}`
var p Person
err := json.Unmarshal([]byte(input), &p)
// p.Name = "Scott", p.Age = 30
```

### Must use pointer — what happens without `&`

```go
var p Person
err := json.Unmarshal([]byte(input), p)  // missing & — RUNTIME ERROR
```

Without `&`, `Unmarshal` receives a **copy** of `p`, not `p` itself. It tries to write to the copy, which can't modify the original. Go returns a runtime error:

```
json: Unmarshal(non-pointer main.Person)
```

`json.Unmarshal` **requires** a pointer. Same for `json.NewDecoder(r.Body).Decode(&v)`.

---

## Common patterns

```go
// Reading JSON from HTTP request body
var req CreateURLRequest
err := json.NewDecoder(r.Body).Decode(&req)

// Writing JSON to HTTP response
w.Header().Set("Content-Type", "application/json")
json.NewEncoder(w).Encode(response)
```

`json.NewDecoder` reads from any `io.Reader` (like `r.Body`).
`json.NewEncoder` writes to any `io.Writer` (like `w`).

No need to manually create `[]byte` — the encoder/decoder handles streaming.

---

## Senior mindset: "Use `json.NewDecoder` for requests, `json.NewEncoder` for responses"

```go
// ❌ Reading then unmarshalling
body, _ := io.ReadAll(r.Body)
json.Unmarshal(body, &data)

// ✅ Streaming directly
json.NewDecoder(r.Body).Decode(&data)
```

The decoder version:
- Handles large bodies without loading everything into memory
- Is one line less code
- Is the idiomatic Go pattern

Always use decoder/encoder when working with HTTP. Use marshal/unmarshal when working with files or strings.

### Empty body — returns `io.EOF`, not zero values

```go
var req CreateURLRequest
err := json.NewDecoder(r.Body).Decode(&req)
```

If the request body is empty (no bytes at all), `Decode` returns **`io.EOF`** — not a struct with zero values.

The decoder expects at least one JSON token. An empty body hits end-of-file immediately, before any JSON is read. Always check the error.

Same with `json.Unmarshal([]byte(""), &p)` — returns `io.ErrUnexpectedEndOfJSON` (a different error, but still an error, not a valid decode).

---


## Summary

| Operation | Code |
|-----------|------|
| Struct → JSON bytes | `json.Marshal(s)` |
| JSON bytes → struct | `json.Unmarshal(b, &s)` |
| JSON from request | `json.NewDecoder(r.Body).Decode(&v)` |
| JSON to response | `json.NewEncoder(w).Encode(v)` |
| Omit zero field | `json:"field,omitempty"` |
| Skip field entirely | `json:"-"` |

## Checklist: did it stick?

- Can you explain `json.Marshal` vs `json.NewEncoder` without looking? (Marshal = bytes, NewEncoder = writer)
- What does `omitempty` do? (Skips the field if it's zero — empty string, 0, false, nil)
- What happens with empty body + `Decode`? (Returns `io.EOF`)
- What happens without `&` in `Unmarshal`? (Runtime error: "non-pointer")
- Why set `Content-Type: application/json`? (Tells the client to parse as JSON)

Open DAY12/DRILL.md.
