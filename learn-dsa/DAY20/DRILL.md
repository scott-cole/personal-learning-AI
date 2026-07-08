# DRILL — BFS and DFS

## Task

Implement BFS (to find shortest path) and DFS on the same graph.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Graph struct {
    vertices int
    edges    [][]int
}

func NewGraph(v int) *Graph {
    g := &Graph{vertices: v, edges: make([][]int, v)}
    return g
}

func (g *Graph) AddEdge(u, v int) {
    g.edges[u] = append(g.edges[u], v)
    g.edges[v] = append(g.edges[v], u)
}

// TODO: BFS traversal from start, return visited order.
func BFS(g *Graph, start int) []int {
    return nil
}

// TODO: BFS shortest path from start to target. Return the path as a slice of vertices.
func ShortestPath(g *Graph, start, target int) []int {
    return nil
}

// TODO: DFS traversal from start, return visited order.
func DFS(g *Graph, start int) []int {
    return nil
}

func main() {
    g := NewGraph(7)
    g.AddEdge(0, 1)
    g.AddEdge(0, 2)
    g.AddEdge(1, 3)
    g.AddEdge(1, 4)
    g.AddEdge(2, 5)
    g.AddEdge(2, 6)

    fmt.Println("BFS from 0:", BFS(g, 0))
    fmt.Println("DFS from 0:", DFS(g, 0))
    fmt.Println("Shortest path 0→6:", ShortestPath(g, 0, 6))
    fmt.Println("Shortest path 0→4:", ShortestPath(g, 0, 4))
}
```

## Expected Output

```
BFS from 0: [0 1 2 3 4 5 6]
DFS from 0: [0 1 3 4 2 5 6] (or similar, order may vary)
Shortest path 0→6: [0 2 6]
Shortest path 0→4: [0 1 4]
```

## Hints

- BFS: use a queue slice. Visit neighbours in order.
- ShortestPath: track `parent[]` during BFS. Reconstruct from target back to start.
- DFS: recursive or use a stack. Mark visited when you pop (iterative) or when you visit (recursive).

## Run

```bash
go run main.go
```
