# CHECKPOINT — Singly Linked Lists

1. What is the time complexity of inserting at the beginning of a singly linked list?
   <details>
   O(1) — just update the head pointer.
   </details>

2. Why is appending O(n) in a singly linked list?
   <details>
   You must traverse from head to tail to find the last node.
   </details>

3. How do you reverse a linked list?
   <details>
   Iterate with prev/curr/next pointers, reversing each node's Next pointer to point to prev.
   </details>

4. What is the main disadvantage of a singly linked list vs an array?
   <details>
   No O(1) random access. Finding any element by index requires O(n) traversal.
   </details>

5. Why does deletion require O(n) to find the node but O(1) once found?
   <details>
   Finding the target node requires scanning. But once you have the node, updating pointers is O(1).
   </details>
