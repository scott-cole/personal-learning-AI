# DAY 10 — Stacks

## A Pile of Plates

A stack of plates in a cafeteria: you put a clean plate on top, and you take a plate from the top. You never pull a plate from the middle — that would be chaos. This is LIFO (Last In, First Out). The last plate you put on is the first one someone grabs.

Stacks are fundamental in programming: function calls (the call stack), undo/redo, expression evaluation, backtracking.

## Slice-Based Stack

```go
type Stack struct {
    items []int
}

func (s *Stack) Push(value int) {
    s.items = append(s.items, value)
}

func (s *Stack) Pop() (int, bool) {
    if len(s.items) == 0 {
        return 0, false
    }
    val := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return val, true
}

func (s *Stack) Peek() (int, bool) {
    if len(s.items) == 0 {
        return 0, false
    }
    return s.items[len(s.items)-1], true
}

func (s *Stack) IsEmpty() bool {
    return len(s.items) == 0
}

func (s *Stack) Size() int {
    return len(s.items)
}
```

## Linked-List-Based Stack

```go
type StackNode struct {
    Value int
    Next  *StackNode
}

type LinkedStack struct {
    Top *StackNode
    len int
}

func (s *LinkedStack) Push(value int) {
    s.Top = &StackNode{Value: value, Next: s.Top}
    s.len++
}

func (s *LinkedStack) Pop() (int, bool) {
    if s.Top == nil {
        return 0, false
    }
    val := s.Top.Value
    s.Top = s.Top.Next
    s.len--
    return val, true
}
```

## Real-World Use: Balanced Parentheses

```go
func isValid(s string) bool {
    pairs := map[rune]rune{')': '(', '}': '{', ']': '['}
    var stack []rune
    for _, ch := range s {
        switch ch {
        case '(', '{', '[':
            stack = append(stack, ch)
        case ')', '}', ']':
            if len(stack) == 0 || stack[len(stack)-1] != pairs[ch] {
                return false
            }
            stack = stack[:len(stack)-1]
        }
    }
    return len(stack) == 0
}
```

## Summary

| Operation | Slice Stack | Linked Stack |
|---|---|---|
| Push | O(1) amortised | O(1) |
| Pop | O(1) | O(1) |
| Peek | O(1) | O(1) |
| Space overhead | Backing array | Per-node pointers |
