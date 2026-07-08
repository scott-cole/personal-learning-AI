# DRILL — Singly Linked List

## Task

Implement a singly linked list with insert, delete, search, and reverse.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Node struct {
    Value int
    Next  *Node
}

type LinkedList struct {
    Head *Node
}

// TODO: Insert value at the beginning.
func (l *LinkedList) Prepend(value int) {
}

// TODO: Insert value at the end.
func (l *LinkedList) Append(value int) {
}

// TODO: Delete the first node with the given value.
func (l *LinkedList) Delete(value int) {
}

// TODO: Return true if value exists.
func (l *LinkedList) Search(value int) bool {
    return false
}

// TODO: Reverse the list in-place.
func (l *LinkedList) Reverse() {
}

func (l *LinkedList) Print() {
    for curr := l.Head; curr != nil; curr = curr.Next {
        fmt.Print(curr.Value)
        if curr.Next != nil {
            fmt.Print(" -> ")
        }
    }
    fmt.Println()
}

func main() {
    list := &LinkedList{}
    list.Prepend(1)
    list.Prepend(2)
    list.Append(3)
    list.Print()

    list.Delete(1)
    list.Print()

    fmt.Println("Search 2:", list.Search(2))
    fmt.Println("Search 99:", list.Search(99))

    list.Reverse()
    list.Print()
}
```

## Expected Output

```
2 -> 1 -> 3
2 -> 3
Search 2: true
Search 99: false
3 -> 2
```

## Hints

- Prepend: new node points to current head, head becomes new node.
- Delete: handle head deletion separately.
- Reverse: three pointers — prev, curr, next.

## Run

```bash
go run main.go
```
