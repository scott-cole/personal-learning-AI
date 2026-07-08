# DAY 08 — Singly Linked Lists

## A Treasure Hunt

A treasure hunt has clues. Each clue tells you where to find the next one. You start at clue #1, follow it to clue #2, and so on until the treasure. If you lose a clue, you're stuck. If you want to insert a new clue between #3 and #4, you just rewrite #3's pointer — the other clues stay where they are.

That's a singly linked list: each node holds a value and a pointer to the next node. You can only traverse forward. Insertion and deletion are O(1) if you already have the right node, but finding a node is O(n).

## The Node

```go
type Node struct {
    Value int
    Next  *Node
}

type LinkedList struct {
    Head *Node
}
```

## Operations

```go
// Insert at the beginning — O(1)
func (l *LinkedList) Prepend(value int) {
    l.Head = &Node{Value: value, Next: l.Head}
}

// Insert at the end — O(n)
func (l *LinkedList) Append(value int) {
    node := &Node{Value: value}
    if l.Head == nil {
        l.Head = node
        return
    }
    curr := l.Head
    for curr.Next != nil {
        curr = curr.Next
    }
    curr.Next = node
}

// Delete by value — O(n)
func (l *LinkedList) Delete(value int) {
    if l.Head == nil {
        return
    }
    if l.Head.Value == value {
        l.Head = l.Head.Next
        return
    }
    curr := l.Head
    for curr.Next != nil && curr.Next.Value != value {
        curr = curr.Next
    }
    if curr.Next != nil {
        curr.Next = curr.Next.Next
    }
}

// Search — O(n)
func (l *LinkedList) Search(value int) bool {
    curr := l.Head
    for curr != nil {
        if curr.Value == value {
            return true
        }
        curr = curr.Next
    }
    return false
}
```

## Reversing a Linked List

```go
func (l *LinkedList) Reverse() {
    var prev *Node
    curr := l.Head
    for curr != nil {
        next := curr.Next
        curr.Next = prev
        prev = curr
        curr = next
    }
    l.Head = prev
}
```

## Traversal

```go
func (l *LinkedList) Print() {
    curr := l.Head
    for curr != nil {
        fmt.Print(curr.Value)
        if curr.Next != nil {
            fmt.Print(" -> ")
        }
        curr = curr.Next
    }
    fmt.Println()
}
```

## Summary

| Operation | Time |
|---|---|
| Prepend | O(1) |
| Append | O(n) |
| Insert after node | O(1) (if you have the node) |
| Delete (head) | O(1) |
| Delete (value) | O(n) |
| Search | O(n) |
| Reverse | O(n) |
