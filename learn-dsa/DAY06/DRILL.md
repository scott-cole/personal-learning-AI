# DRILL — In-Place Operations

## Task

Implement three in-place array operations.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Rotate nums to the right by k steps in-place.
func Rotate(nums []int, k int) {
}

// TODO: Sort an array containing only 0, 1, 2 in-place.
func SortColors(nums []int) {
}

// TODO: Move all elements equal to val to the end. Return the count of non-val elements.
func RemoveElement(nums []int, val int) int {
    return 0
}

func main() {
    // Rotate
    a := []int{1, 2, 3, 4, 5, 6, 7}
    Rotate(a, 3)
    fmt.Println("rotate 3:", a)

    // Sort colors
    b := []int{2, 0, 2, 1, 1, 0}
    SortColors(b)
    fmt.Println("sort colors:", b)

    // Remove element
    c := []int{3, 2, 2, 3}
    k := RemoveElement(c, 3)
    fmt.Println("remove 3:", c[:k])
}
```

## Expected Output

```
rotate 3: [5 6 7 1 2 3 4]
sort colors: [0 0 1 1 2 2]
remove 3: [2 2]
```

## Hints

- Rotate: use the triple-reverse technique.
- SortColors: Dutch National Flag — three pointers.
- RemoveElement: slow pointer tracks where to write.

## Run

```bash
go run main.go
```
