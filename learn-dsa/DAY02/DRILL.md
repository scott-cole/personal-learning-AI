# DRILL — Array and Slice Operations

## Task

Implement slice operations from scratch.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Insert val at index idx in slice s. Return the new slice.
func Insert(s []int, idx int, val int) []int {
    return nil
}

// TODO: Delete the element at index idx from slice s. Return the new slice.
func Delete(s []int, idx int) []int {
    return nil
}

// TODO: Reverse the slice in-place.
func Reverse(s []int) {
}

func main() {
    s := []int{1, 2, 3, 4, 5}
    fmt.Println("original:", s)

    s = Insert(s, 2, 99)
    fmt.Println("after insert 99 at 2:", s)

    s = Delete(s, 3)
    fmt.Println("after delete at 3:", s)

    Reverse(s)
    fmt.Println("after reverse:", s)
}
```

## Expected Output

```
original: [1 2 3 4 5]
after insert 99 at 2: [1 2 99 3 4 5]
after delete at 3: [1 2 99 4 5]
after reverse: [5 4 99 2 1]
```

## Hints

- Use `append` and `copy` for insertion.
- For deletion, `append` the parts before and after the index.
- Reversal: swap first with last, second with second-last, etc.

## Run

```bash
go run main.go
```
