# CHECKPOINT — Binary Trees

1. What are the three depth-first traversal orders?
   <details>
   Preorder (root → left → right), Inorder (left → root → right), Postorder (left → right → root).
   </details>

2. What data structure is used for level-order traversal?
   <details>
   A queue (FIFO). BFS on a tree uses a queue.
   </details>

3. What is the maximum number of nodes in a binary tree of height h?
   <details>
   2^h - 1 (a perfect binary tree). Height is number of levels.
   </details>

4. What traversal gives sorted order in a BST?
   <details>
   Inorder (left → root → right).
   </details>

5. What is the height of a tree with only a root node?
   <details>
   1 (or 0 depending on convention — be consistent). Most definitions: height of a single node = 1.
   </details>
