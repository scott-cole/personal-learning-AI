# DAY 18 — Heaps

## A Tournament Bracket

In a knockout tournament, 16 players start. In round 1, winners advance. Round 2 has 8, round 3 has 4, and the final has 2. The champion is at the top. If the champion withdraws, the next best player takes their place — but you need to re-run some matches.

A heap is exactly this: a binary tree where every parent is greater (max-heap) or smaller (min-heap) than its children. The root is always the extreme element. Insertion and extraction both take O(log n).

## Array Representation

A heap is stored as an array where the children of index `i` are at `2i+1` (left) and `2i+2` (right).

```go
//       100
//      /   \
//     50    30
//    /  \   /
//   10  20 5

// As array: [100, 50, 30, 10, 20, 5]
// Parent of i: (i-1)/2
// Left child:   2i+1
// Right child:  2i+2
```

## Heap Operations

```go
type MinHeap struct {
    items []int
}

func (h *MinHeap) Push(val int) {
    h.items = append(h.items, val)
    h.siftUp(len(h.items) - 1)
}

func (h *MinHeap) Pop() (int, bool) {
    if len(h.items) == 0 {
        return 0, false
    }
    min := h.items[0]
    h.items[0] = h.items[len(h.items)-1]
    h.items = h.items[:len(h.items)-1]
    h.siftDown(0)
    return min, true
}

func (h *MinHeap) siftUp(i int) {
    for i > 0 {
        parent := (i - 1) / 2
        if h.items[parent] <= h.items[i] {
            break
        }
        h.items[parent], h.items[i] = h.items[i], h.items[parent]
        i = parent
    }
}

func (h *MinHeap) siftDown(i int) {
    n := len(h.items)
    for {
        smallest := i
        left := 2*i + 1
        right := 2*i + 2
        if left < n && h.items[left] < h.items[smallest] {
            smallest = left
        }
        if right < n && h.items[right] < h.items[smallest] {
            smallest = right
        }
        if smallest == i {
            break
        }
        h.items[i], h.items[smallest] = h.items[smallest], h.items[i]
        i = smallest
    }
}
```

## Heapify — Build Heap in O(n)

```go
func (h *MinHeap) Heapify(nums []int) {
    h.items = nums
    n := len(nums)
    for i := n/2 - 1; i >= 0; i-- {
        h.siftDown(i)
    }
}
```

## Using container/heap (review)

```go
type IntHeap []int
func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] }
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }
func (h *IntHeap) Push(x any)        { *h = append(*h, x.(int)) }
func (h *IntHeap) Pop() any          { old := *h; x := old[len(old)-1]; *h = old[:len(old)-1]; return x }
```

## Summary

| Operation | Time |
|---|---|
| Push (insert) | O(log n) |
| Pop (extract min/max) | O(log n) |
| Peek (top) | O(1) |
| Heapify | O(n) |
