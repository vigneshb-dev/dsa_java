# Heap & Priority Queue

> Complete binary tree satisfying heap order property for constant-time extreme element access.

## 1. Overview
A Heap is a specialized tree-based structure satisfying the heap invariant: in a min-heap, every parent is less than or equal to its children. It is typically implemented as a compact array where child indices are calculated arithmetically (`2*i + 1`, `2*i + 2`). Java's `PriorityQueue` provides an unbounded binary min-heap by default, customizable with a `Comparator`.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `peek()` (get min/max) | O(1) / O(1) / O(1) | O(1) |
| `offer()` (insert) | O(1) / O(log n) / O(log n) | O(1) |
| `poll()` (extract min/max) | O(1) / O(log n) / O(log n) | O(1) |
| `remove(Object)` | O(n) / O(n) / O(n) | O(1) |
| Heapify (build from array) | O(n) / O(n) / O(n) | O(1) auxiliary |

Building a heap from an array of size n via bottom-up sift-down takes linear O(n) time, not O(n log n).

## 3. When to Use
- Finding Top-K largest or smallest elements in a streaming or static dataset.
- Dijkstra's shortest path and Prim's minimum spanning tree algorithms.
- Merging K sorted lists or streams efficiently.
- Interval scheduling where earliest ending or starting events must be tracked.

## 4. When NOT to Use
- Arbitrary element search is needed (heaps take O(n) for searching non-root items).
- Sorted traversal of all items is required frequently (heaps are only partially ordered).
- Frequent arbitrary item updates without holding index handles (removal is O(n)).

## 5. Why It Works
Because the root always stores the global minimum (or maximum), peek is immediate O(1). Extracting the root and moving the last leaf to the top requires at most log2(n) sift-down steps, restoring the invariant in O(log n).

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Sort Entire Array) | Sort array completely to get top K elements | O(n log n) | O(1) or O(n) |
| Optimized (Min-Heap of Size K) | Maintain K-size min-heap; evict root when size > K | O(n log k) | O(k) |

Maintaining a bounded heap of size k avoids sorting all n elements, dropping time to O(n log k).

## 7. Data Structures Used Here
- `PriorityQueue<E>`: Java standard library priority queue backed by a dynamically resized `Object[]` array.
- `Comparator<T>`: Used to reverse order (for max-heap: `Collections.reverseOrder()`) or custom priority logic.

## 8. Core Template (Java)
```java
// Min-heap (default in Java)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();
minHeap.offer(5);
minHeap.offer(1);
int smallest = minHeap.poll(); // 1

// Max-heap using custom comparator
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
maxHeap.offer(5);
maxHeap.offer(1);
int largest = maxHeap.poll(); // 5

// Top K pattern skeleton (keep top K largest elements)
for (int num : nums) {
    minHeap.offer(num);
    if (minHeap.size() > k) {
        minHeap.poll();
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Balanced Trees](../10-balanced-trees/README.md) | [Trie](../12-trie/README.md)
