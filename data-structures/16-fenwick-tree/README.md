# Fenwick Tree (Binary Indexed Tree)

> Compact array-based structure for prefix sums and point updates using bitwise lowest set bits.

## 1. Overview
A Fenwick Tree, or Binary Indexed Tree (BIT), is a data structure that efficiently updates elements and calculates prefix sums in a table of numbers. Unlike a Segment Tree which requires 4*n storage and explicit tree recursion, a Fenwick Tree uses exactly n + 1 memory elements and clean iterative bitwise operations. Each index `i` stores the sum of a range whose length equals `i & (-i)`, the lowest set bit of `i`.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Point Update (add delta) | O(log n) / O(log n) / O(log n) | O(1) |
| Prefix Sum query `[1..i]` | O(log n) / O(log n) / O(log n) | O(1) |
| Range Sum query `[l..r]` | O(log n) / O(log n) / O(log n) | O(1) |
| Build (from array) | O(n) / O(n) / O(n) | O(n) |

A Fenwick Tree requires only 1-indexed array of length n + 1, offering superior cache locality and smaller constants than segment trees.

## 3. When to Use
- Dynamic prefix sums where array elements are frequently modified by point increments.
- Range sum queries calculated via `prefixSum(r) - prefixSum(l - 1)`.
- Counting inversions in an array (using coordinate compression + frequency BIT).
- When minimal code length and low constant factors are preferred over segment tree versatility.

## 4. When NOT to Use
- Range minimum/maximum queries without invertible operations on arbitrary intervals (prefer Segment Tree).
- Arbitrary range updates and range queries simultaneously without dual BIT complexity.
- Static datasets where simple prefix sum arrays provide O(1) queries.

## 5. Why It Works
Every integer has a unique binary representation. The operation `i & (-i)` extracts the lowest power of 2 that divides `i`. Subtracting `i & (-i)` strips the lowest set bit to hop to the next non-overlapping prefix sum block, reaching 0 in at most log2(n) steps.

## 6. Brute Force vs Optimized
| Approach | Idea | Update Time | Range Sum Time |
| :--- | :--- | :--- | :--- |
| Prefix Sum Array | Rebuild prefix array upon update | O(n) | O(1) |
| Fenwick Tree (BIT) | Update ancestor bit intervals and sum prefix bit intervals | O(log n) | O(log n) |

The Fenwick Tree balances query and update costs at O(log n) without the high memory footprint of segment trees.

## 7. Data Structures Used Here
- `int[] tree`: 1-based array of size `n + 1` storing interval partial sums.

## 8. Core Template (Java)
```java
// Standard Fenwick Tree (Binary Indexed Tree) skeleton
class FenwickTree {
    private final int[] tree;
    private final int n;

    public FenwickTree(int n) {
        this.n = n;
        this.tree = new int[n + 1];
    }

    public void add(int i, int delta) { // 1-based index
        for (; i <= n; i += i & -i) {
            tree[i] += delta;
        }
    }

    public int query(int i) { // prefix sum [1..i]
        int sum = 0;
        for (; i > 0; i -= i & -i) {
            sum += tree[i];
        }
        return sum;
    }

    public int queryRange(int l, int r) {
        return query(r) - query(l - 1);
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Segment Tree](../15-segment-tree/README.md) | [Design Problems](../17-design-problems/README.md)
