# DRILL — Backtracking

## Task

Implement N-Queens solver using backtracking.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

// TODO: Solve N-Queens and return all valid board configurations.
// Each board is a []string where 'Q' marks a queen and '.' marks empty.
func SolveNQueens(n int) [][]string {
    return nil
}

func printBoards(boards [][]string) {
    for i, board := range boards {
        fmt.Printf("Solution %d:\n", i+1)
        for _, row := range board {
            fmt.Println(" ", row)
        }
        fmt.Println()
    }
}

func main() {
    boards := SolveNQueens(4)
    fmt.Printf("Found %d solutions for 4-Queens:\n", len(boards))
    printBoards(boards)

    boards8 := SolveNQueens(8)
    fmt.Printf("Found %d solutions for 8-Queens\n", len(boards8))
}
```

## Expected Output

```
Found 2 solutions for 4-Queens:
Solution 1:
  .Q..
  ...Q
  Q...
  ..Q.

Solution 2:
  ..Q.
  Q...
  ...Q
  .Q..

Found 92 solutions for 8-Queens
```

## Hints

- Track columns, diagonal1 (row+col), diagonal2 (row-col+n-1) as `[]bool`.
- Place queens row by row. For each row, try every column.
- If no queen attacks the position, place and recurse to the next row.

## Run

```bash
go run main.go
```
