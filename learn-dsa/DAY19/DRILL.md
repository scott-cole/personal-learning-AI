# DRILL — Graph Representations

## Task

Build an undirected graph with an adjacency list, add edges, and implement degree and neighbour queries.

## Starter Code

Create `main.go`:

```go
package main

import "fmt"

type Graph struct {
    vertices int
    edges    [][]int
}

// TODO: Create a new graph with v vertices.
func NewGraph(v int) *Graph {
    return nil
}

// TODO: Add an undirected edge between u and v.
func (g *Graph) AddEdge(u, v int) {
}

// TODO: Return the degree (number of neighbours) of vertex v.
func (g *Graph) Degree(v int) int {
    return 0
}

// TODO: Return true if v is a neighbour of u.
func (g *Graph) HasEdge(u, v int) bool {
    return false
}

func main() {
    g := NewGraph(5)
    g.AddEdge(0, 1)
    g.AddEdge(0, 2)
    g.AddEdge(1, 3)
    g.AddEdge(2, 4)

    for i := 0; i < 5; i++ {
        fmt.Printf("Vertex %d (degree %d): %v\n", i, g.Degree(i), g.edges[i])
    }

    fmt.Println("Has edge 0-1:", g.HasEdge(0, 1))
    fmt.Println("Has edge 1-4:", g.HasEdge(1, 4))
}
```

## Expected Output

```
Vertex 0 (degree 2): [1 2]
Vertex 1 (degree 2): [0 3]
Vertex 2 (degree 2): [0 4]
Vertex 3 (degree 1): [1]
Vertex 4 (degree 1): [2]
Has edge 0-1: true
Has edge 1-4: false
```

## Hints

- For AddEdge on an undirected graph, add v to u's list AND u to v's list.
- HasEdge: iterate through g.edges[u] and check.

## Run

```bash
go run main.go
```
