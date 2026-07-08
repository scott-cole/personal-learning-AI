# CHECKPOINT — Graphs

1. What are the two standard ways to represent a graph?
   <details>
   Adjacency list (slice of slices) and adjacency matrix (2D array of bools/ints).
   </details>

2. When would you use an adjacency matrix over an adjacency list?
   <details>
   When the graph is dense (many edges) or you need O(1) edge queries.
   </details>

3. What is the space complexity of an adjacency list?
   <details>
   O(V + E) — each vertex and each edge is stored once.
   </details>

4. What is the degree of a vertex?
   <details>
   The number of edges incident to that vertex. In an undirected graph, it's the length of the adjacency list.
   </details>

5. How do you add an edge in a directed graph vs undirected?
   <details>
   Directed: add v to u's list only. Undirected: add v to u's list and u to v's list.
   </details>
