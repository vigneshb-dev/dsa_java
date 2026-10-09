# Heap & Priority Queue
> Complete binary tree enforcing the heap-order property, providing O(1) extremum access and O(log n) insertions and extractions.

## 1. Fundamentals
- What is it? A complete binary tree where every parent node satisfies the heap property relative to its children (parent $\le$ children in a Min-Heap; parent $\ge$ children in a Max-Heap).
- What problem does it solve? Efficiently retrieves and removes the highest/lowest priority element without paying the cost of full sorting.
- What type of data does it store? Comparable keys and priority items (`Comparable` or `Comparator`).
- Linear or non-linear? Non-linear (complete binary tree abstraction implemented directly over a linear contiguous array).
- Static or dynamic? Dynamic: dynamically expands its internal array buffer upon reaching capacity.
- Ordered or unordered? Partially ordered (weak ordering): extremum is at the root, but siblings and subtrees are not sorted relative to each other.
- Mutable or immutable? Mutable in Java: states update dynamically via sift-up and sift-down operations.
- How is the data stored internally? Packed contiguously in a 0-indexed array where node $i$ has parent $\lfloor(i - 1)/2\rfloor$, left child $2i + 1$, and right child $2i + 2$.
```text
Binary Min-Heap Array and Tree Layout:
Array indices: [0]=2, [1]=4, [2]=7, [3]=8, [4]=10, [5]=15

Tree structure:
             [ 2 ] (index 0)
            /     \
   (idx 1) [ 4 ]   [ 7 ] (idx 2)
          /    \   /
 (idx 3)[ 8 ] [10][15] (idx 5)
      (idx 4)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `offer(e)` or `push(e)`. Place new element at next free array slot (index `size`).
  2. Sift-up (swim): Compare new element with parent at `(i - 1) / 2`.
  3. If new element violates heap order, swap with parent and repeat until root or order restored.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(n) |

Note: Average insertion into a random binary heap takes O(1) because most elements settle in the lowest levels; worst-case sift-up to root takes O(log n).

### Delete
- **How it works:**
  1. Often named `poll()` or `extractMin()`. Save root element `arr[0]`.
  2. Overwrite root with the last element `arr[size - 1]`, nullify last slot, decrement size.
  3. Sift-down (sink): Compare node against smaller child; if child is smaller than parent, swap downward. Repeat until heap property holds.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Removing the root extremum takes $O(\log n)$ due to sinking down tree height.

### Search
- **How it works:**
  1. Because a heap is only partially ordered, binary search cannot be applied.
  2. Perform a linear scan through the underlying backing array comparing each element.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Arbitrary search is $O(n)$; indexed priority queues augment the heap with a hash map to achieve $O(1)$ search.

### Access
- **How it works:**
  1. Often named `peek()`. Check if size > 0.
  2. Read and return the root element at `arr[0]` directly without removing it.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Accessing the minimum (or maximum) element is strictly $O(1)$.

### Update
- **How it works:**
  1. If element index is known: update `arr[index] = newValue`.
  2. If new value is smaller (in min-heap), sift-up; if larger, sift-down.
  3. If element index is unknown, finding it takes $O(n)$ before updating.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Update at a known index takes $O(\log n)$ to restore heap order.

### Traverse
- **How it works:**
  1. Iterate index from 0 to `size - 1` across backing array.
  2. Note: backing array iteration yields level-order heap order, NOT sorted order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Direct traversal does not produce elements in sorted order.

### Sort
- **How it works:**
  1. Heapify input array in-place in $O(n)$ time using bottom-up sift-downs.
  2. For $i = n - 1$ down to 1: swap root `arr[0]` with `arr[i]`, and sift-down root in range $[0, i - 1]$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(1) |
| Average | O(n log n) | O(1) |
| Worst | O(n log n) | O(1) |

Note: Heapsort sorts completely in-place in $O(n \log n)$ time with $O(1)$ auxiliary space.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Offer) | O(1) | O(1) | O(log n) | O(1) |
| Delete (Poll) | O(1) | O(log n) | O(log n) | O(1) |
| Search | O(1) | O(n) | O(n) | O(1) |
| Access (Peek) | O(1) | O(1) | O(1) | O(1) |
| Update (Known Idx)| O(1) | O(log n) | O(log n) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort (Heapsort) | O(n log n) | O(n log n) | O(n log n) | O(1) |

## 3. Variations
- **Standard version:** Binary Min-Heap (contiguous array-backed). Trade-off: Unmatched cache efficiency and $O(1)$ peek, but $O(n)$ search for non-root elements and $O(n)$ to merge two heaps.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Max-Heap | Root holds maximum element; parent $\ge$ children | Immediate access to largest key | Cannot access minimum in O(1) | Finding top-K smallest, median finding |
| d-ary Heap | Each node has $d$ children instead of 2 | Flatter tree (shallower height); cache friendly | Slower sift-down (needs $d$ child comparisons) | Dijkstra's algorithm on dense graphs (4-ary) |
| Indexed Priority Queue | Maintains inverse position map (`qp[id] = index`) | O(1) key lookup and O(log n) decrease-key | Additional array overhead and index management | Prim's, Dijkstra's algorithm |
| Fibonacci Heap | Lazy multi-tree forest structure | O(1) amortized decrease-key and merge | Enormous constant factors; highly complex | Theoretical network flow algorithms |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Array-Backed Binary Heap | Flat 1D array using arithmetic index calculation | Optimal spatial memory locality; zero node headers | Pre-allocated buffer capacity overhead |
| Pointer-Linked Heap | Nodes with explicit parent, left, and right pointers | Eliminates resizing array reallocations | High reference overhead; poor CPU cache usage |
| D-ary Array Heap ($d=4$) | Flat array with children at $4i + 1$ to $4i + 4$ | Reduces sift-up comparisons and memory hops | Sift-down compares 4 child keys per level |

- **Java built-in equivalents:**
  - `java.util.PriorityQueue`: Unbounded min-heap backed by `Object[] queue`; grows by 50-100% on capacity breach.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyPriorityQueue.java` | Array-backed Min/Max Heap | Dynamic array binary heap with bottom-up heapify, sift-up, and sift-down |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [10 - Balanced Trees](../10-balanced-trees/README.md) | [12 - Trie](../12-trie/README.md)
