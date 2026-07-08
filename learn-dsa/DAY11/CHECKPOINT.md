# CHECKPOINT — Queues

1. What does FIFO stand for?
   <details>
   First In, First Out — the oldest element is removed first.
   </details>

2. Why is dequeue on a slice-based queue O(n)?
   <details>
   Removing the first element shifts all remaining elements left by one.
   </details>

3. How does a linked-list queue achieve O(1) dequeue?
   <details>
   By maintaining both Front and Back pointers. Dequeue just moves Front forward.
   </details>

4. What problem does a ring buffer solve?
   <details>
   It avoids memory reallocation and O(n) shifting by wrapping around a fixed-size array using modular arithmetic.
   </details>

5. When would you use a deque (double-ended queue)?
   <details>
   When you need to insert or remove from both ends efficiently — e.g., sliding window max, palindrome checking.
   </details>
