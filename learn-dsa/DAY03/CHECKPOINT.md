# CHECKPOINT — Two Pointers

1. What is the time complexity of the two-pointer pair sum on a sorted array?
   <details>
   O(n). Each pointer moves at most n steps total.
   </details>

2. Why does the two-pointer approach work for pair sum on sorted arrays?
   <details>
   Because moving left increases the sum, moving right decreases it. We narrow in on the target.
   </details>

3. In the palindrome check, when do the left and right pointers stop?
   <details>
   When they meet or cross (left >= right).
   </details>

4. What is the slow/fast pattern used for besides removing duplicates?
   <details>
   Cycle detection in linked lists (Floyd's tortoise and hare).
   </details>

5. Can two pointers work on an unsorted array for pair sum?
   <details>
   Only if you sort first (O(n log n)) or use a hash map (O(n) average).
   </details>
