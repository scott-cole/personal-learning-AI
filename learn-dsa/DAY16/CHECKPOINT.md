# CHECKPOINT — Binary Search Trees

1. What property must a BST satisfy?
   <details>
   For every node, all values in the left subtree are smaller, and all values in the right subtree are larger.
   </details>

2. What is the time complexity of search in a balanced BST?
   <details>
   O(log n), where n is the number of nodes.
   </details>

3. What happens if you insert sorted values (1, 2, 3, ...) into a BST?
   <details>
   The tree degenerates into a linked list — O(n) operations instead of O(log n).
   </details>

4. In delete with two children, what node replaces the deleted node?
   <details>
   The inorder successor — the smallest node in the right subtree.
   </details>

5. What traversal of a BST prints values in sorted order?
   <details>
   Inorder (left → root → right).
   </details>
