# DAY 09 — Doubly Linked Lists

## A Two-Way Street

A one-way street: you can only drive forward. Need to go back? You have to go around the block. A two-way street lets you drive in both directions — you can always reverse course.

A singly linked list is the one-way street. A doubly linked list adds a `Prev` pointer so you can traverse backwards too. This makes operations at the tail O(1) — just keep a `Tail` pointer and walk backwards.

## The Node

```go
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
```

## Insert at Head — O(1)

```go
func (l *DoublyLinkedList) Prepend(value int) {
    node := &Node{Value: value}
    if l.Head == nil {
        l.Head = node
        l.Tail = node
    } else {
        node.Next = l.Head
        l.Head.Prev = node
        l.Head = node
    }
    l.Size++
}
```

## Insert at Tail — O(1)

```go
func (l *DoublyLinkedList) Append(value int) {
    node := &Node{Value: value}
    if l.Tail == nil {
        l.Head = node
        l.Tail = node
    } else {
        node.Prev = l.Tail
        l.Tail.Next = node
        l.Tail = node
    }
    l.Size++
}
```

## Delete at Head — O(1)

```go
func (l *DoublyLinkedList) DeleteHead() {
    if l.Head == nil {
        return
    }
    l.Head = l.Head.Next
    if l.Head != nil {
        l.Head.Prev = nil
    } else {
        l.Tail = nil
    }
    l.Size--
}
```

## Delete at Tail — O(1)

```go
func (l *DoublyLinkedList) DeleteTail() {
    if l.Tail == nil {
        return
    }
    l.Tail = l.Tail.Prev
    if l.Tail != nil {
        l.Tail.Next = nil
    } else {
        l.Head = nil
    }
    l.Size--
}
```

## Forward and Backward Traversal

```go
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
```

## Summary

| Operation | Singly | Doubly |
|---|---|---|
| Prepend | O(1) | O(1) |
| Append | O(n) | O(1) |
| Delete head | O(1) | O(1) |
| Delete tail | O(n) | O(1) |
| Backward traversal | Not possible | O(n) |
| Memory per node | 1 pointer | 2 pointers |
