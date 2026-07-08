# CHECKPOINT — Sliding Window

1. What is the time complexity of brute-force maximum sum subarray of size k (summing every window from scratch)?
   <details>
   O(n·k). Each of the n windows sums k elements.
   </details>

2. How does the sliding window version reduce this?
   <details>
   It reuses the previous sum: add the new element, subtract the old one. O(n).
   </details>

3. In the longest-substring problem, when does the left pointer move?
   <details>
   When a character repeats within the current window.
   </details>

4. What data structure is commonly used to track characters in the window?
   <details>
   A map (hash table) from character to its latest index.
   </details>

5. Can sliding window solve problems involving subarrays with a sum ≥ target?
   <details>
   Yes — that's a dynamic window that shrinks when the sum exceeds the target.
   </details>
