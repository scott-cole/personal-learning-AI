# DRILL — Queue

## Task

Implement a linked-list-based queue with enqueue, dequeue, and peek.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type QueueNode struct {
    Value int
    Next  *QueueNode
}

type Queue struct {
    Front *QueueNode
    Back  *QueueNode
    Size  int
}

// TODO: Add value to the back of the queue.
func (q *Queue) Enqueue(value int) {
}

// TODO: Remove and return the front value.
func (q *Queue) Dequeue() (int, bool) {
    return 0, false
}

// TODO: Return the front value without removing it.
func (q *Queue) Peek() (int, bool) {
    return 0, false
}

// TODO: Return true if the queue is empty.
func (q *Queue) IsEmpty() bool {
    return false
}

func main() {
    q := &Queue{}
    q.Enqueue(10)
    q.Enqueue(20)
    q.Enqueue(30)

    fmt.Println("Dequeue:", must(q.Dequeue()))
    fmt.Println("Peek:", must(q.Peek()))
    fmt.Println("Dequeue:", must(q.Dequeue()))
    fmt.Println("IsEmpty:", q.IsEmpty())
    fmt.Println("Dequeue:", must(q.Dequeue()))
    fmt.Println("IsEmpty:", q.IsEmpty())
}

func must(val int, ok bool) int {
    if !ok {
        panic("unexpected empty queue")
    }
    return val
}
```

## Expected Output

```
Dequeue: 10
Peek: 20
Dequeue: 20
IsEmpty: false
Dequeue: 30
IsEmpty: true
```

## Hints

- Enqueue: if back is nil, set front and back to new node. Otherwise, link the new node after back and update back.
- Dequeue: move front forward. If front becomes nil, also set back to nil.
- Both ends are O(1).

## Run

```bash
go run main.go
```
