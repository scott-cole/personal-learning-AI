# CHECKPOINT — Trade-Offs

1. Why is prepending to a slice O(n)?
   <details>
   All existing elements must be shifted right by one position.
   </details>

2. Why is iterating a slice faster than iterating a linked list?
   <details>
   Slice elements are contiguous in memory (cache-friendly). Linked list nodes are scattered (pointer chasing).
   </details>

3. What is the main reason to choose a linked list over an array?
   <details>
   O(1) insertion/deletion at arbitrary positions when you already have a reference to the node.
   </details>

4. Does Go's `list.List` implement a singly or doubly linked list?
   <details>
   Doubly linked list — each node has Prev and Next pointers.
   </details>

5. In Go production code, which data structure is more common for sequences?
   <details>
   Slices. They are idiomatic, efficient, and versatile. Linked lists are rarely the right choice in Go.
   </details>
