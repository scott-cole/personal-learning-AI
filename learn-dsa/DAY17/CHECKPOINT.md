# CHECKPOINT — Tree Recursion

1. What is the base case for most recursive tree functions?
   <details>
   `if root == nil { return ... }` — an empty tree is the smallest subproblem.
   </details>

2. In `maxDepth`, what does the recursive case return?
   <details>
   `1 + max(maxDepth(left), maxDepth(right))` — add 1 for the current node.
   </details>

3. What makes a binary tree height-balanced?
   <details>
   For every node, the height difference between left and right subtrees is at most 1.
   </details>

4. What traversal does `invertTree` use?
   <details>
   Preorder — it swaps children before recursing. Postorder would also work (swap after recursion).
   </details>

5. In serialization, why are nil markers needed?
   <details>
   To reconstruct the exact tree shape. Without markers, different trees could produce the same sequence.
   </details>
