# DAY 22 — Hash Maps

## A Valet Parking Lot

A valet parking lot has numbered spots. When you hand over your keys, the valet gives you a ticket with a number. You don't remember the spot — you remember the ticket. Hand the ticket back, and the valet retrieves your car instantly.

A hash map works the same: you give it a key (the ticket), it computes a hash (the spot number), and retrieves the value (the car). Good hash maps give O(1) average lookups.

## Hash Function

A hash function maps a key to an integer index. A good hash is:
- **Deterministic**: same key → same hash
- **Fast**: O(1) to compute
- **Uniform**: spreads keys evenly across buckets

```go
func hash(key string, capacity int) int {
    h := 0
    for _, ch := range key {
        h = 31*h + int(ch)
    }
    if h < 0 {
        h = -h
    }
    return h % capacity
}
```

## Separate Chaining (Collision Handling)

When two keys hash to the same index, we store them in a linked list at that index.

```go
type KeyValue struct {
    Key   string
    Value int
}

type HashMap struct {
    buckets [][]KeyValue
    size    int
}

func NewHashMap(capacity int) *HashMap {
    return &HashMap{
        buckets: make([][]KeyValue, capacity),
    }
}

func (m *HashMap) Put(key string, value int) {
    idx := hash(key, len(m.buckets))
    // Check if key exists, update it
    for i, kv := range m.buckets[idx] {
        if kv.Key == key {
            m.buckets[idx][i].Value = value
            return
        }
    }
    // Key doesn't exist, append
    m.buckets[idx] = append(m.buckets[idx], KeyValue{key, value})
    m.size++
}

func (m *HashMap) Get(key string) (int, bool) {
    idx := hash(key, len(m.buckets))
    for _, kv := range m.buckets[idx] {
        if kv.Key == key {
            return kv.Value, true
        }
    }
    return 0, false
}

func (m *HashMap) Delete(key string) {
    idx := hash(key, len(m.buckets))
    bucket := m.buckets[idx]
    for i, kv := range bucket {
        if kv.Key == key {
            m.buckets[idx] = append(bucket[:i], bucket[i+1:]...)
            m.size--
            return
        }
    }
}
```

## Load Factor and Resizing

When `size / capacity` exceeds a threshold (e.g., 0.75), double capacity and rehash all entries.

```go
func (m *HashMap) resize() {
    oldBuckets := m.buckets
    m.buckets = make([][]KeyValue, len(oldBuckets)*2)
    m.size = 0
    for _, bucket := range oldBuckets {
        for _, kv := range bucket {
            m.Put(kv.Key, kv.Value)
        }
    }
}
```

## Built-in map

Go has a built-in hash map (`map[string]int`). You should use it in production. But building one from scratch teaches you how collisions, hashing, and resizing work.

## Summary

| Operation | Average | Worst |
|---|---|---|
| Put | O(1) | O(n) |
| Get | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Space | O(n) | O(n) |

Worst case happens when all keys collide (bad hash function or adversarial input).
