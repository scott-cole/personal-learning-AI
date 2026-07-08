# DAY 02 — Arrays and Slices

## Numbered Apartment Mailboxes

An apartment building has 100 mailboxes, each with a fixed number. Mailbox #7 never moves to a different spot — it's always in slot 7. That's an array: a fixed-size block of slots, each at a known position.

A `[5]int` in Go is exactly that — five numbered boxes. But buildings also expand: your neighbour moves out and a new one moves in. That's a slice: a dynamic array that can grow or shrink.

Arrays in Go are **fixed-size** values. Slices are **views** into arrays — they have a length and a capacity.

## Arrays

```go
var nums [5]int        // [0 0 0 0 0]
nums[0] = 42
fmt.Println(nums[0])   // 42
fmt.Println(len(nums)) // 5
```

## Slices

```go
// Declaration
var s []int               // nil slice, len=0
s = []int{1, 2, 3}        // literal
s = make([]int, 5)        // len=5, cap=5
s = make([]int, 3, 10)    // len=3, cap=10

// Append (amortised O(1))
s = append(s, 4)

// Slice expressions
arr := [5]int{10, 20, 30, 40, 50}
slice := arr[1:4]          // [20, 30, 40]
```

## Traversal, Insertion, Deletion

```go
// Traversal — O(n)
for i, v := range slice {
    fmt.Println(i, v)
}

// Insert at position — O(n)
func insert(s []int, idx, val int) []int {
    s = append(s, 0)           // make room
    copy(s[idx+1:], s[idx:])   // shift right
    s[idx] = val
    return s
}

// Delete at position — O(n)
func delete(s []int, idx int) []int {
    return append(s[:idx], s[idx+1:]...)
}
```

## Zero Value

```go
var arr [3]int    // [0 0 0]
var s []string    // nil (len=0, cap=0)
```

A nil slice has no backing array. It's safe to `append` to nil — Go allocates one for you.

## Capacity and Growth

A slice grows by doubling its capacity (for small sizes) when it runs out of room. This ensures amortised O(1) append.

```go
s := make([]int, 0, 2)
s = append(s, 1) // cap=2, len=1
s = append(s, 2) // cap=2, len=2
s = append(s, 3) // cap=4, len=3 — new backing array!
```

## Summary

| Operation | Array | Slice |
|---|---|---|
| Access by index | O(1) | O(1) |
| Append | N/A | O(1) amortised |
| Insert | N/A | O(n) |
| Delete | N/A | O(n) |
| Fixed size | Yes | No |
