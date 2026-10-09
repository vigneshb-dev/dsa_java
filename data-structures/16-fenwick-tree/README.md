# Fenwick Tree
> Binary Indexed Tree (BIT) maintaining dynamic prefix sums and point updates in O(log n) time using minimal O(n) space and bitwise indexing.

## 1. Fundamentals
- What is it? An array-based data structure that stores cumulative partial sums using binary least significant bit (LSB) indexing.
- What problem does it solve? Answers dynamic prefix sum queries and executes point updates in $O(\log n)$ time with negligible code complexity and exactly $O(n)$ memory.
- What type of data does it store? Invertible associative values (such as numeric additions, counts, or XOR sums).
- Linear or non-linear? Non-linear (implicit binary tree structure mapped directly into a linear 1D array).
- Static or dynamic? Static in capacity $n$; dynamic in value updates.
- Ordered or unordered? Strictly ordered by 1-based sequential indices.
- Mutable or immutable? Mutable in Java: cell values update dynamically through bitwise index chains.
- How is the data stored internally? Packed into a 1-indexed flat primitive array `int[] tree` of size $n + 1$, where slot $i$ stores the aggregate of range $(i - \text{LSB}(i), i]$ with $\text{LSB}(i) = i \ \& \ (-i)$.
```text
Fenwick Tree LSB Interval Coverage (1-indexed):
Idx 1 (001): [1..1]
Idx 2 (010): [1..2]  (covers 1 to 2)
Idx 3 (011): [3..3]
Idx 4 (100): [1..4]  (covers 1 to 4)
Idx 5 (101): [5..5]
Idx 6 (110): [5..6]  (covers 5 to 6)
Idx 7 (111): [7..7]
Idx 8 (1000):[1..8]  (covers 1 to 8)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `add(i, delta)`.
  2. Start at index $i$ (1-indexed).
  3. While $i \le n$: add `delta` to `tree[i]`, and advance index by adding its least significant bit: `i += i & (-i)`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Traverses the binary bit-chain upward in at most $\lfloor\log_2 n\rfloor + 1$ iterations.

### Delete
- **How it works:**
  1. Subtracting an element's value is accomplished by adding the negative value.
  2. Execute `add(i, -val)`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Removing an entry's influence takes identical steps to insertion ($O(\log n)$).

### Search
- **How it works:**
  1. Prefix Sum Query: `query(i)`. Initialize `sum = 0`.
  2. While $i > 0$: add `tree[i]` to `sum`, and strip the least significant bit: `i -= i & (-i)`.
  3. Range Sum Query: calculate `query(r) - query(l - 1)`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Best-case occurs when $i$ is a power of 2 (resolves in 1 step); worst case visits number of set bits in $i$.

### Access
- **How it works:**
  1. To read the single original element at index $i$:
  2. Evaluate `query(i) - query(i - 1)` in $2 \times O(\log n)$ operations.
  3. Alternatively, retain original array `arr[]` for instant $O(1)$ read access.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Isolated point access is derived from two prefix queries ($O(\log n)$).

### Update
- **How it works:**
  1. To update index $i$ to `newVal`:
  2. Compute difference `delta = newVal - get(i)`.
  3. Call `add(i, delta)`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(log n) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: In-place value reassignment takes $O(\log n)$.

### Traverse
- **How it works:**
  1. Iterate across tree array from index 1 to $n$.
  2. Inspect each partial sum bucket.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Linear scan visits all $n$ bucket slots.

### Sort
- **How it works:**
  1. Fenwick tree stores spatial sequence sums, not ordered values.
  2. N/A: Sorting values is not applicable (though a Fenwick tree over coordinate frequencies can compute inversions and find order statistics in $O(\log n)$).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | N/A | N/A |
| Average | N/A | N/A |
| Worst | N/A | N/A |

Note: Sorting is N/A because BIT operates on array positional indices.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Point Add (Insert)| O(log n) | O(log n) | O(log n) | O(1) |
| Point Delete | O(log n) | O(log n) | O(log n) | O(1) |
| Prefix Sum Query | O(1) | O(log n) | O(log n) | O(1) |
| Range Sum Query | O(1) | O(log n) | O(log n) | O(1) |
| Point Access | O(log n) | O(log n) | O(log n) | O(1) |
| Point Update | O(log n) | O(log n) | O(log n) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort | N/A | N/A | N/A | N/A |

## 3. Variations
- **Standard version:** 1D Point Update, Range Sum BIT. Trade-off: Fastest execution and minimal $O(n)$ space, but requires invertible operations (cannot easily handle arbitrary range minimum/maximum queries).
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Range Update, Point Query | Stores differences in BIT; queries prefix to get point value | $O(\log n)$ range additions without lazy tags | Range queries require dual tree tracking | Dynamic difference arrays |
| Range Update, Range Query | Uses two BITs: one for $d_i$ and one for $d_i \times (i - 1)$ | Full range updates and range sums | Slightly more complex algebraic formulas | Full dynamic interval arithmetic |
| 2D Fenwick Tree | Nested loops manipulating 2D array `tree[R][C]` | $O(\log R \log C)$ 2D subgrid sum queries | Memory scales to $O(R \times C)$ | 2D dynamic matrix subgrid sums |
| Inversion Counting BIT | Operates on frequency of elements over sorted ranks | Counts inversions in $O(n \log n)$ total time | Requires coordinate compression for large numbers | Permutation inversion count |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Fenwick Tree (BIT) | Flat array with bitwise LSB arithmetic `i += i & (-i)` | Extremely fast; zero branching overhead | Exactly $n + 1$ integers ($O(n)$ space) |
| Segment Tree | Explicit or implicit binary tree dividing ranges into halves | Handles non-invertible operations (min, max, gcd) | $4n$ integers ($4\times$ memory footprint) |

- **Java built-in equivalents:**
  - Standard Java has no built-in `FenwickTree` or `BinaryIndexedTree` class in `java.util`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyFenwickTree.java` | 1D Binary Indexed Tree | Prefix sums, range sums, point updates, and binary lifting search |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [15 - Segment Tree](../15-segment-tree/README.md) | [17 - Design Problems](../17-design-problems/README.md)
