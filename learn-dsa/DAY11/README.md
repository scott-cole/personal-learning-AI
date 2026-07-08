# DAY 11 — Queues

## A Line at the Checkout

When you join a checkout queue at the supermarket, you stand behind the last person. The cashier serves the person at the front. New people join at the back, and the person at the front leaves when served. This is FIFO (First In, First Out).

Queues are essential for BFS (breadth-first search), task scheduling, buffering, and any "first come, first served" scenario.

## Slice-Based Queue

```go
type Queue struct {
    items []int
}

func (q *Queue) Enqueue(value int) {
    q.items = append(q.items, value)
}

func (q *Queue) Dequeue() (int, bool) {
    if len(q.items) == 0 {
        return 0, false
    }
    val := q.items[0]
    q.items = q.items[1:]
    return val, true
}

func (q *Queue) Peek() (int, bool) {
    if len(q.items) == 0 {
        return 0, false
    }
    return q.items[0], true
}

func (q *Queue) IsEmpty() bool {
    return len(q.items) == 0
}
```

**Warning**: slice-based dequeue is O(n) because it shifts all elements. For performance, use a ring buffer or linked list.

## Linked-List-Based Queue

```go
type QueueNode struct {
    Value int
    Next  *QueueNode
}

type LinkedQueue struct {
    Front *QueueNode
    Back  *QueueNode
    Size int
}

func (q *LinkedQueue) Enqueue(value int) {
    node := &QueueNode{Value: value}
    if q.Back != nil {
        q.Back.Next = node
    }
    q.Back = node
    if q.Front == nil {
        q.Front = node
    }
    q.Size++
}

func (q *LinkedQueue) Dequeue() (int, bool) {
    if q.Front == nil {
        return 0, false
    }
    val := q.Front.Value
    q.Front = q.Front.Next
    if q.Front == nil {
        q.Back = nil
    }
    q.Size--
    return val, true
}
```

## Ring Buffer Queue (Circular Queue)

```go
type RingQueue struct {
    buf  []int
    head int
    tail int
    size int
    cap  int
}

func NewRingQueue(capacity int) *RingQueue {
    return &RingQueue{buf: make([]int, capacity), cap: capacity}
}

func (q *RingQueue) Enqueue(value int) bool {
    if q.size == q.cap {
        return false
    }
    q.buf[q.tail] = value
    q.tail = (q.tail + 1) % q.cap
    q.size++
    return true
}

func (q *RingQueue) Dequeue() (int, bool) {
    if q.size == 0 {
        return 0, false
    }
    val := q.buf[q.head]
    q.head = (q.head + 1) % q.cap
    q.size--
    return val, true
}
```

## Summary

| Operation | Slice Queue | Linked Queue | Ring Queue |
|---|---|---|---|
| Enqueue | O(1) amortised | O(1) | O(1) |
| Dequeue | O(n) | O(1) | O(1) |
| Memory | Contiguous | Per-node pointers | Fixed size |
