# CHECKPOINT — Priority Queues

1. What is the time complexity of Push and Pop on a heap?
   <details>
   Both are O(log n). Peek (accessing the top) is O(1).
   </details>

2. How do you turn a min-heap into a max-heap in Go?
   <details>
   Change the `Less` method: for max-heap, return `h[i] > h[j]`.
   </details>

3. Why must you use `heap.Push` and `heap.Pop` instead of calling the methods directly?
   <details>
   The `heap` package methods maintain the heap invariant (sift up/down). Direct calls would skip this.
   </details>

4. What is the heap invariant for a min-heap?
   <details>
   Every parent is ≤ its children. The minimum is always at the root (index 0).
   </details>

5. What is the time complexity of `heap.Init`?
   <details>
   O(n) — it uses Floyd's heap construction algorithm, which is more efficient than n individual pushes.
   </details>
