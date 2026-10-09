# Segment Tree
> Complete binary tree structure storing interval aggregates to support logarithmic range queries and range updates.

## 1. Fundamentals
- What is it? A binary tree where each node represents an interval aggregate (sum, min, max, GCD) over a segment of an array.
- What problem does it solve? Answers arbitrary range queries and processes point/range modifications in $O(\log n)$ time where static prefix sums fail due to dynamic updates.
- What type of data does it store? Associative algebraic aggregates (monoid values) over discrete numeric segments.
- Linear or non-linear? Non-linear (binary interval tree).
- Static or dynamic? Static in segment boundary size $n$; dynamic when implemented with pointer-based node creation over huge coordinate domains (e.g. $[0, 10^9]$).
- Ordered or unordered? Strictly ordered by interval indices $[L, R]$.
- Mutable or immutable? Mutable in Java: aggregates and lazy tags update dynamically.
- How is the data stored internally? Packed in an array of size $4n$ where node $i$ has left child $2i + 1$ and right child $2i + 2$, or via pointer-linked `Node` objects.
```text
Segment Tree Layout over array [2, 3, 4, 5] (Range Sum):
                  [0..3] (Sum: 14)
                 /                \
        [0..1] (Sum: 5)        [2..3] (Sum: 9)
        /            \         /            \
   [0..0] (2)    [1..1] (3)  [2..2] (4)   [3..3] (5)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `build(arr, node, l, r)`.
  2. If $l == r$, initialize leaf with `tree[node] = arr[l]`.
  3. Otherwise, recursively build left and right children, then merge results: `tree[node] = merge(tree[2*node+1], tree[2*node+2])`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Building the tree bottom-up evaluates all $2n - 1$ tree nodes in linear $O(n)$ time.

### Delete
- **How it works:**
  1. In a fixed segment tree, removing an element corresponds to updating that position to the identity value (e.g., 0 for sum, $+\infty$ for min).
  2. Descend to the target leaf and re-compute parent aggregates upward.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(log n) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Modifying a leaf back to identity takes $O(\log n)$ path updates.

### Search
- **How it works:**
  1. Often named `query(node, l, r, ql, qr)`.
  2. If current segment $[l, r]$ is completely inside query $[ql, qr]$, return `tree[node]`.
  3. If completely disjoint, return identity value.
  4. Otherwise, push down pending lazy updates, split query to both children, and merge results.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Decomposes any arbitrary range into at most $2 \lceil\log_2 n\rceil$ canonical node intervals.

### Access
- **How it works:**
  1. Accessing single element at index $i$ is a point query: `query(i, i)`.
  2. Traverses single downward path from root to leaf $i$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Accessing a single element through the tree takes $O(\log n)$; raw array access remains $O(1)$ if the raw array is retained.

### Update
- **How it works:**
  1. Point Update: Descend path to target leaf, modify value, recalculate parent aggregates on the return path in $O(\log n)$.
  2. Range Update (Lazy Propagation): Tag intermediate node with pending change without descending further; push tag down only when children are accessed.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Range updates achieve $O(\log n)$ strictly when paired with Lazy Propagation.

### Traverse
- **How it works:**
  1. Standard pre-order or level-order tree traversal across all internal and leaf nodes.
  2. Iterates across the backing array slots from index 0 to $4n$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(log n) |
| Average | O(n) | O(log n) |
| Worst | O(n) | O(log n) |

Note: Complete traversal visits all $O(n)$ nodes.

### Sort
- **How it works:**
  1. A segment tree maintains position-based segments, not value-sorted elements.
  2. N/A: Sorting the underlying values is not the responsibility of a standard segment tree (though an order-statistic segment tree can locate the k-th smallest element in $O(\log n)$).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | N/A | N/A |
| Average | N/A | N/A |
| Worst | N/A | N/A |

Note: Sorting is N/A because nodes represent positional index ranges.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Build (Insert All)| O(n) | O(n) | O(n) | O(n) |
| Point Update | O(log n) | O(log n) | O(log n) | O(log n) |
| Range Update (Lazy)| O(log n) | O(log n) | O(log n) | O(log n) |
| Range Query | O(1) | O(log n) | O(log n) | O(log n) |
| Point Access | O(log n) | O(log n) | O(log n) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(log n) |
| Sort | N/A | N/A | N/A | N/A |

## 3. Variations
- **Standard version:** Array-based Recursive Segment Tree (`int[4n]`). Trade-off: Versatile support for arbitrary monoids and clean range queries, but $4n$ memory multiplier and recursive call overhead.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Iterative Segment Tree | Flat array of size $2n$; non-recursive loops | 2x less memory; 2-3x faster execution | Complex to apply lazy propagation | Point updates with range queries |
| Lazy Propagation Segment Tree | Delays segment sub-updates via lazy tags | $O(\log n)$ range updates and range queries | Requires careful tag pushing and merging | Range addition/assignment queries |
| Dynamic / Sparse Segment Tree | Nodes allocated dynamically with left/right pointers | Handles vast coordinate ranges up to $10^9$ | Pointer overhead; higher memory per node | Large ranges with sparse coordinates |
| Persistent Segment Tree | Creates new path of $O(\log n)$ nodes per update | Preserves all historical versions of the tree | Consumes $O(q \log n)$ total memory | Range k-th smallest element, 2D range queries |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Array-Based Recursive | `tree[4n]` indexed via $2i + 1$ and $2i + 2$ | Clean code; recursive function call overhead | $4n$ elements array buffer |
| Iterative Bottom-Up | `tree[2n]` where leaves reside at $[n, 2n - 1]$ | Fast CPU loop; zero recursion overhead | Exactly $2n$ elements array buffer |
| Pointer-Based Dynamic | Nodes contain `Node left`, `Node right` references | Allocates only visited intervals on demand | High node reference memory footprint |

- **Java built-in equivalents:**
  - Standard Java has no built-in `SegmentTree` class in `java.util`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MySegmentTree.java` | Range Sum Segment Tree with Lazy Propagation | Array-backed segment tree with range query, point update, and lazy range add |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [14 - Union-Find](../14-union-find/README.md) | [16 - Fenwick Tree](../16-fenwick-tree/README.md)
