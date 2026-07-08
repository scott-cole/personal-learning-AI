# DRILL — Hash Map from Scratch

## Task

Implement a hash map with separate chaining, including Put, Get, Delete.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type KeyValue struct {
    Key   string
    Value int
}

type HashMap struct {
    buckets [][]KeyValue
    size    int
}

// TODO: Create a new hash map with the given capacity.
func NewHashMap(capacity int) *HashMap {
    return nil
}

// TODO: Hash function for strings.
func hash(key string, capacity int) int {
    return 0
}

// TODO: Insert or update key-value pair.
func (m *HashMap) Put(key string, value int) {
}

// TODO: Retrieve value by key.
func (m *HashMap) Get(key string) (int, bool) {
    return 0, false
}

// TODO: Delete key-value pair.
func (m *HashMap) Delete(key string) {
}

func main() {
    m := NewHashMap(8)
    m.Put("alice", 100)
    m.Put("bob", 200)
    m.Put("charlie", 300)

    fmt.Println("alice:", must(m.Get("alice")))
    fmt.Println("bob:", must(m.Get("bob")))
    fmt.Println("unknown:", func() string {
        _, ok := m.Get("unknown")
        if !ok {
            return "not found"
        }
        return "found"
    }())

    m.Delete("alice")
    fmt.Println("alice after delete:", func() string {
        _, ok := m.Get("alice")
        if !ok {
            return "not found"
        }
        return "found"
    }())

    m.Put("bob", 999)
    fmt.Println("bob after update:", must(m.Get("bob")))
}

func must(val int, ok bool) int {
    if !ok {
        panic("key not found")
    }
    return val
}
```

## Expected Output

```
alice: 100
bob: 200
unknown: not found
alice after delete: not found
bob after update: 999
```

## Hints

- Hash: iterate over characters, multiply by 31, add char code, take mod at the end.
- Put: hash the key, scan bucket for existing key (update) or append.
- Get/Delete: hash the key, scan bucket.

## Run

```bash
go run main.go
```
