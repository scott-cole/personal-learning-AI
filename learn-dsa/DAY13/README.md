# DAY 13 — Trade-Offs: Array vs Linked List

## Choosing Between a Sports Car and a Pickup Truck

A sports car is fast, sleek, and great for the highway. But try moving a sofa with it. A pickup truck is slower, bulkier, and worse on gas — but you can haul lumber, dirt bikes, and furniture.

Arrays and linked lists are the same: each is terrible at what the other does well. Choosing between them depends on what operations you need most.

## Arrays (Contiguous Memory)

```
Stack memory: [████] [░░░░] [▓▓▓▓] [▒▒▒▒]
Index access: arr[2] → base + 2·sizeof(int) → instant
```

**Strengths:**
- O(1) random access by index
- Cache-friendly (contiguous memory)
- No per-element pointer overhead
- Excellent for iteration

**Weaknesses:**
- O(n) insertion/deletion (shifting)
- Fixed size (slices handle this with amortised growth)
- Insert at front is O(n)

## Linked Lists (Non-Contiguous)

```
Scattered memory: [███]→[▓▓▓]→[▒▒▒]→[░░░]
```

**Strengths:**
- O(1) insertion/deletion at known position
- Never needs resizing
- Always O(1) prepend
- Easy to split/merge lists

**Weaknesses:**
- O(n) random access
- Cache- unfriendly (pointer chasing)
- Extra memory per element (1-2 pointers)
- More complex code

## Decision Matrix

| Scenario | Array | Linked List |
|---|---|---|
| Read by index frequently | ✓ | ✗ |
| Insert/delete at ends | ✓ (append only) | ✓ |
| Insert/delete at arbitrary position | ✗ | ✓ (if you have the node) |
| Memory efficiency (dense data) | ✓ | ✗ |
| Predictable performance | ✓ | ✓ |
| Large elements | ✓ (stores pointers) | ✓ |
| CPU cache efficiency | ✓ | ✗ |

## Go-Specific Considerations

```go
// Arrays and slices are almost always the right default in Go.
// Use linked lists when:
// 1. You need O(1) insertion in the middle and already have a pointer
// 2. You're implementing a pure functional data structure
// 3. The standard library uses one (list.List, ring.Ring)
```

Go's `container/list` provides a doubly linked list. It's rarely used in production Go code compared to slices.

## Summary

- **Array (slice)** wins for random access, iteration, and cache efficiency.
- **Linked list** wins for frequent insert/delete at arbitrary positions.
- In practice, slices are the go-to. Profile before replacing with a linked list.
