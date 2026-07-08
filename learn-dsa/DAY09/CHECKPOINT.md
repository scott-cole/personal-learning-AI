# CHECKPOINT — Doubly Linked Lists

1. What extra field does a doubly linked list node have compared to a singly linked list?
   <details>
   A `Prev` pointer pointing to the previous node.
   </details>

2. Why can a doubly linked list append in O(1)?
   <details>
   Because it maintains a Tail pointer. You insert directly after the tail.
   </details>

3. What is the memory overhead trade-off of a doubly linked list?
   <details>
   Each node stores two pointers instead of one. More memory, but more flexible operations.
   </details>

4. In `DeleteHead`, why must you check if the list becomes empty?
   <details>
   If the list had one node, both Head and Tail must be set to nil after deletion.
   </details>

5. Can you delete the tail in O(1) in a singly linked list?
   <details>
   No — you need to traverse to find the node before tail, which is O(n).
   </details>
