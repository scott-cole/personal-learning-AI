# DRILL — Priority Queue

## Task

Implement a priority queue using Go's `container/heap` to solve a task scheduler problem.

## Starter Code

Create `main.go`:

```go
package main

import (
    "container/heap"
    "fmt"
)

type Task struct {
    Name     string
    Priority int
}

// TODO: Implement heap.Interface for a max-priority queue of Tasks.
type PriorityQueue []*Task

func (pq PriorityQueue) Len() int { return 0 }

// TODO: Less — higher priority first.
func (pq PriorityQueue) Less(i, j int) bool {
    return false
}

func (pq PriorityQueue) Swap(i, j int) {
}

func (pq *PriorityQueue) Push(x any) {
}

func (pq *PriorityQueue) Pop() any {
    return nil
}

func main() {
    pq := &PriorityQueue{}
    heap.Init(pq)

    tasks := []Task{
        {"Write code", 3},
        {"Fix critical bug", 5},
        {"Check email", 1},
        {"Design review", 4},
        {"Nap", 2},
    }

    for _, t := range tasks {
        heap.Push(pq, &Task{Name: t.Name, Priority: t.Priority})
    }

    fmt.Println("Processing tasks by priority:")
    for pq.Len() > 0 {
        task := heap.Pop(pq).(*Task)
        fmt.Printf("  %s (priority %d)\n", task.Name, task.Priority)
    }
}
```

## Expected Output

```
Processing tasks by priority:
  Fix critical bug (priority 5)
  Design review (priority 4)
  Write code (priority 3)
  Nap (priority 2)
  Check email (priority 1)
```

## Hints

- `Less` should return `pq[i].Priority > pq[j].Priority`.
- `Push` appends `x.(*Task)`.
- `Pop` removes and returns the last element.
- Always use `heap.Push` and `heap.Pop`, not the methods directly.

## Run

```bash
go run main.go
```
