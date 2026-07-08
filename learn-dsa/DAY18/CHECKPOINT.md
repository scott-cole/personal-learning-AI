# CHECKPOINT — Heaps

1. In an array-based heap, where is the parent of index i?
   <details>
   At index (i-1)/2 (integer division).
   </details>

2. What is the time complexity of heapify?
   <details>
   O(n) — Floyd's algorithm. Most nodes are near the bottom and need few swaps.
   </details>

3. What happens during siftDown?
   <details>
   A node is swapped with its smaller child until it's smaller than both children (or it's a leaf).
   </details>

4. What is the difference between a min-heap and a max-heap?
   <details>
   In a min-heap, the root is the smallest element. In a max-heap, the root is the largest.
   </details>

5. Can you use a heap to sort an array?
   <details>
   Yes — heap sort. Heapify then repeatedly pop the root. O(n log n).
   </details>
