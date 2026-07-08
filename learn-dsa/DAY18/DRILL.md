# DRILL — Min-Heap from Scratch

## Task

Implement a min-heap with push, pop, and heapify.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type MinHeap struct {
    items []int
}

// TODO: Push a value into the heap.
func (h *MinHeap) Push(val int) {
}

// TODO: Pop the minimum value from the heap.
func (h *MinHeap) Pop() (int, bool) {
    return 0, false
}

// TODO: Peek at the minimum value without removing it.
func (h *MinHeap) Peek() (int, bool) {
    return 0, false
}

// TODO: Build the heap from an arbitrary slice in O(n).
func (h *MinHeap) Heapify(nums []int) {
}

func main() {
    h := &MinHeap{}
    h.Heapify([]int{9, 4, 7, 1, 3, 8, 2, 5})
    fmt.Println("Peek:", must(h.Peek()))

    h.Push(0)
    fmt.Println("After push 0, peek:", must(h.Peek()))

    sorted := make([]int, 0, 9)
    for h.Peek() != (int{}) {
        val, ok := h.Pop()
        if !ok {
            break
        }
        sorted = append(sorted, val)
    }
    fmt.Println("Sorted:", sorted)
}

func must(val int, ok bool) int {
    if !ok {
        panic("empty heap")
    }
    return val
}
```

## Expected Output

```
Peek: 1
After push 0, peek: 0
Sorted: [0 1 2 3 4 5 7 8 9]
```

## Hints

- siftUp: while the node is smaller than its parent, swap.
- siftDown: while the node is larger than its smallest child, swap with that child.
- Heapify: start from the last non-leaf node (n/2 - 1) and siftDown each.

## Run

```bash
go run main.go
```
