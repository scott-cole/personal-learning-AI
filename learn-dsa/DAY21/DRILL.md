# DRILL — Maze Solver

## Task

Implement BFS and DFS maze solvers that find a path from S to E.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Point struct {
    Row, Col int
}

// TODO: Find shortest path from S to E using BFS.
// Return the path as a slice of Points (including S and E).
// If no path exists, return nil.
func SolveBFS(maze [][]int, start, end Point) []Point {
    return nil
}

// TODO: Find any path from S to E using DFS.
func SolveDFS(maze [][]int, start, end Point) []Point {
    return nil
}

func main() {
    maze := [][]int{
        {0, 0, 1, 0, 0},
        {0, 0, 1, 0, 0},
        {1, 0, 0, 0, 1},
        {0, 0, 1, 0, 0},
        {0, 1, 0, 0, 0},
    }

    start := Point{0, 0}
    end := Point{4, 4}

    fmt.Println("BFS path:")
    path := SolveBFS(maze, start, end)
    if path != nil {
        for _, p := range path {
            fmt.Printf("  (%d,%d)", p.Row, p.Col)
        }
        fmt.Println()
        fmt.Println("Length:", len(path)-1)
    } else {
        fmt.Println("  No path found")
    }

    fmt.Println("DFS path:")
    path2 := SolveDFS(maze, start, end)
    if path2 != nil {
        for _, p := range path2 {
            fmt.Printf("  (%d,%d)", p.Row, p.Col)
        }
        fmt.Println()
        fmt.Println("Length:", len(path2)-1)
    } else {
        fmt.Println("  No path found")
    }
}
```

## Expected Output

```
BFS path:
  (0,0) (0,1) (1,1) (2,1) (2,2) (2,3) (3,3) (4,3) (4,4)
Length: 8
DFS path:
  (0,0) (0,1) (1,1) (2,1) (2,2) (2,3) (3,3) (4,3) (4,4)
Length: 8
```

(DFS path may differ — BFS finds shortest, DFS finds any.)

## Hints

- Directions: up (-1,0), down (1,0), left (0,-1), right (0,1).
- Check bounds: `r >= 0 && r < rows && c >= 0 && c < cols`.
- Only visit cells where `maze[r][c] == 0`.
- For BFS: use a queue and parent map (`map[Point]Point`).
- For DFS: recursion with visited set.

## Run

```bash
go run main.go
```
