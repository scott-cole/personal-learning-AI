# DRILL — Stack

## Task

Implement a stack using a slice, then use it to check balanced parentheses.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Stack struct {
    items []rune
}

// TODO: Push a value onto the stack.
func (s *Stack) Push(value rune) {
}

// TODO: Pop and return the top value.
func (s *Stack) Pop() (rune, bool) {
    return 0, false
}

// TODO: Return true if the stack is empty.
func (s *Stack) IsEmpty() bool {
    return false
}

// TODO: Return true if the parentheses in s are balanced.
// Valid chars: '(', ')', '{', '}', '[', ']'
func IsBalanced(s string) bool {
    return false
}

func main() {
    tests := []string{
        "()",
        "()[]{}",
        "(]",
        "([)]",
        "{[]}",
        "",
    }
    for _, t := range tests {
        fmt.Printf("%-10s -> %v\n", t, IsBalanced(t))
    }
}
```

## Expected Output

```
()         -> true
()[]{}     -> true
(]         -> false
([)]       -> false
{[]}       -> true
           -> true
```

## Hints

- Push opening brackets onto the stack.
- When you see a closing bracket, check if it matches the top.
- If the stack is empty when you pop, it's invalid.

## Run

```bash
go run main.go
```
