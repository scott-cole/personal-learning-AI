# DAY 19 — Graphs

## A Subway Map

A subway map shows stations (nodes) connected by lines (edges). Some connections are direct; others require transfers. Some lines have express trains that skip stations (unweighted) while others take time (weighted). You can model it as an adjacency list: each station has a list of neighbouring stations.

A graph G = (V, E) consists of a set of vertices V and a set of edges E. Graphs can be:
- **Directed** (one-way) vs **undirected** (two-way)
- **Weighted** (edges have costs) vs **unweighted**
- **Cyclic** vs **acyclic**

## Adjacency List

```go
type Graph struct {
    vertices int
    edges    [][]int  // adjacency list
}

func NewGraph(v int) *Graph {
    g := &Graph{
        vertices: v,
        edges:    make([][]int, v),
    }
    return g
}

func (g *Graph) AddEdge(u, v int) {
    g.edges[u] = append(g.edges[u], v)
    g.edges[v] = append(g.edges[v], u) // undirected
}
```

## Adjacency Matrix

```go
type MatrixGraph struct {
    vertices int
    matrix   [][]bool
}

func NewMatrixGraph(v int) *MatrixGraph {
    m := make([][]bool, v)
    for i := range m {
        m[i] = make([]bool, v)
    }
    return &MatrixGraph{vertices: v, matrix: m}
}

func (g *MatrixGraph) AddEdge(u, v int) {
    g.matrix[u][v] = true
    g.matrix[v][u] = true
}
```

## Weighted Graph

```go
type WeightedEdge struct {
    To     int
    Weight int
}

type WeightedGraph struct {
    vertices int
    edges    [][]WeightedEdge
}

func NewWeightedGraph(v int) *WeightedGraph {
    return &WeightedGraph{
        vertices: v,
        edges:    make([][]WeightedEdge, v),
    }
}

func (g *WeightedGraph) AddEdge(u, v, w int) {
    g.edges[u] = append(g.edges[u], WeightedEdge{To: v, Weight: w})
    g.edges[v] = append(g.edges[v], WeightedEdge{To: u, Weight: w})
}
```

## Adjacency List vs Matrix

| Operation | Adjacency List | Adjacency Matrix |
|---|---|---|
| Edge query | O(deg(v)) | O(1) |
| Add edge | O(1) | O(1) |
| Remove edge | O(deg(v)) | O(1) |
| Space | O(V + E) | O(V²) |
| Iterate neighbours | O(deg(v)) | O(V) |
