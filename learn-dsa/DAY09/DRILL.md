# DRILL — Doubly Linked List

## Task

Implement a doubly linked list with insert/delete at both ends.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Node struct {
    Value int
    Next  *Node
    Prev  *Node
}

type DoublyLinkedList struct {
    Head *Node
    Tail *Node
    Size int
}

// TODO: Insert at the beginning.
func (l *DoublyLinkedList) Prepend(value int) {
}

// TODO: Insert at the end.
func (l *DoublyLinkedList) Append(value int) {
}

// TODO: Delete the first element.
func (l *DoublyLinkedList) DeleteHead() {
}

// TODO: Delete the last element.
func (l *DoublyLinkedList) DeleteTail() {
}

func (l *DoublyLinkedList) PrintForward() {
    for curr := l.Head; curr != nil; curr = curr.Next {
        fmt.Print(curr.Value, " ")
    }
    fmt.Println()
}

func (l *DoublyLinkedList) PrintBackward() {
    for curr := l.Tail; curr != nil; curr = curr.Prev {
        fmt.Print(curr.Value, " ")
    }
    fmt.Println()
}

func main() {
    list := &DoublyLinkedList{}
    list.Append(1)
    list.Append(2)
    list.Append(3)
    list.Prepend(0)
    list.PrintForward()
    list.PrintBackward()

    list.DeleteHead()
    list.PrintForward()

    list.DeleteTail()
    list.PrintForward()

    fmt.Println("Size:", list.Size)
}
```

## Expected Output

```
0 1 2 3
3 2 1 0
1 2 3
1 2
Size: 2
```

## Hints

- For Prepend: connect new node ← old head, update head.
- For Append: connect old tail → new node, update tail.
- Always update both head and tail when the list becomes empty.

## Run

```bash
go run main.go
```
