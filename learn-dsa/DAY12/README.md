# DAY 12 — Priority Queues

## ER Triage

In an emergency room, patients don't queue "first come, first served." A heart attack patient goes ahead of someone with a stubbed toe, even if the toe arrived first. The ER uses triage — each patient has a priority, and the highest priority is treated first.

A priority queue works the same way: every element has a priority, and the element with the highest (or lowest) priority is always removed first. In Go, we implement this using `container/heap`.

## How container/heap Works

Go's `container/heap` defines an interface you implement:

```go
type Interface interface {
    sort.Interface  // Len, Less, Swap
    Push(x any)    // append to the end
    Pop() any      // remove and return the last element
}
```

The heap maintains the **heap invariant**: for a min-heap, every parent is ≤ its children.

## Min-Heap (IntHeap)

```go
type IntHeap []int

func (h IntHeap) Len() int           { return len(h) }
func (h IntHeap) Less(i, j int) bool { return h[i] < h[j] } // min-heap
func (h IntHeap) Swap(i, j int)      { h[i], h[j] = h[j], h[i] }

func (h *IntHeap) Push(x any) {
    *h = append(*h, x.(int))
}

func (h *IntHeap) Pop() any {
    old := *h
    n := len(old)
    x := old[n-1]
    *h = old[0 : n-1]
    return x
}
```

## Usage

```go
h := &IntHeap{3, 1, 4, 1, 5}
heap.Init(h)
heap.Push(h, 2)
for h.Len() > 0 {
    fmt.Print(heap.Pop(h))
}
// Output: 1 1 2 3 4 5
```

Always use `heap.Init`, `heap.Push`, `heap.Pop` — never call Push/Pop directly, because they don't maintain the heap invariant.

## Max-Heap

Change `Less` to use `>`:

```go
func (h IntHeap) Less(i, j int) bool { return h[i] > h[j] } // max-heap
```

## Priority Queue with Custom Type

```go
type Item struct {
    Value    string
    Priority int
    index    int  // managed by heap
}

type PriorityQueue []*Item

func (pq PriorityQueue) Len() int { return len(pq) }
func (pq PriorityQueue) Less(i, j int) bool {
    return pq[i].Priority > pq[j].Priority // max priority first
}
func (pq PriorityQueue) Swap(i, j int) {
    pq[i], pq[j] = pq[j], pq[i]
    pq[i].index = i
    pq[j].index = j
}
func (pq *PriorityQueue) Push(x any) {
    n := len(*pq)
    item := x.(*Item)
    item.index = n
    *pq = append(*pq, item)
}
func (pq *PriorityQueue) Pop() any {
    old := *pq
    n := len(old)
    item := old[n-1]
    old[n-1] = nil
    item.index = -1
    *pq = old[0 : n-1]
    return item
}
```

## Summary

| Operation | Time Complexity |
|---|---|
| Insert (Push) | O(log n) |
| Extract (Pop) | O(log n) |
| Peek (top) | O(1) |
| Build heap | O(n) |
