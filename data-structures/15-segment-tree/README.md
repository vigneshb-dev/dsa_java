# Segment Tree

> Tree structure for logarithmic range queries and point/range updates over associative functions.

## 1. Overview
A Segment Tree is a binary tree used for storing intervals or segments, allowing querying which of the stored segments contain a given point or range. It supports range queries (sum, min, max, gcd) and point updates in O(log n) time. When augmented with lazy propagation, it also supports range updates in O(log n) time.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Build Tree | O(n) / O(n) / O(n) | O(n) |
| Point Update | O(log n) / O(log n) / O(log n) | O(1) iterative / O(log n) recursion |
| Range Query (e.g., sum, min) | O(log n) / O(log n) / O(log n) | O(log n) recursion |
| Range Update (with Lazy Propagation) | O(log n) / O(log n) / O(log n) | O(log n) recursion |

A segment tree over n elements requires an array of size up to 4*n (specifically 2^(ceil(log2(n)) + 1) - 1) to accommodate all leaf and internal nodes.

## 3. When to Use
- Frequent range aggregate queries (sum, min, max) combined with frequent point or range updates.
- Dynamic array queries where prefix sums fail because updates occur continuously.
- Range updates (e.g., add V to all elements in range [L, R]) using lazy propagation.
- Coordinate compression queries in 2D geometric problems.

## 4. When NOT to Use
- Array is strictly static with no updates (use simple Prefix Sum array for O(1) queries).
- Only point updates and prefix sums are needed (use Fenwick Tree / BIT, which is simpler and has lower constant factors).
- Query operation is not associative (e.g., median cannot be merged easily across child segments).

## 5. Why It Works
Every internal node stores the pre-computed aggregate of its left and right children. Any arbitrary sub-interval [L, R] decomposes into at most 2 * log2(n) canonical disjoint segments in the tree, allowing logarithmic evaluation.

## 6. Brute Force vs Optimized
| Approach | Idea | Range Query Time | Point Update Time |
| :--- | :--- | :--- | :--- |
| Brute Force (Array Scan) | Iterate from index L to R | O(n) | O(1) |
| Optimized (Segment Tree) | Query canonical tree segments | O(log n) | O(log n) |

The segment tree balances query and update costs so both execute in O(log n) time, avoiding the O(n) bottleneck.

## 7. Data Structures Used Here
- `int[] tree`: 1D array of size `4 * n` storing internal aggregate values.
- `int[] lazy`: Optional array of size `4 * n` holding deferred lazy tags for range updates.

## 8. Core Template (Java)
```java
// Segment Tree for Range Sum with Point Updates
class SegmentTree {
    int n;
    int[] tree;

    public SegmentTree(int[] nums) {
        this.n = nums.length;
        this.tree = new int[4 * n];
        build(nums, 0, 0, n - 1);
    }

    void build(int[] nums, int node, int l, int r) {
        if (l == r) {
            tree[node] = nums[l];
            return;
        }
        int mid = l + (r - l) / 2;
        build(nums, 2 * node + 1, l, mid);
        build(nums, 2 * node + 2, mid + 1, r);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    public void update(int node, int l, int r, int idx, int val) {
        if (l == r) {
            tree[node] = val;
            return;
        }
        int mid = l + (r - l) / 2;
        if (idx <= mid) update(2 * node + 1, l, mid, idx, val);
        else update(2 * node + 2, mid + 1, r, idx, val);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    public int query(int node, int l, int r, int ql, int qr) {
        if (ql <= l && r <= qr) return tree[node];
        if (r < ql || l > qr) return 0;
        int mid = l + (r - l) / 2;
        return query(2 * node + 1, l, mid, ql, qr) + 
               query(2 * node + 2, mid + 1, r, ql, qr);
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Union-Find (Disjoint Set Union)](../14-union-find/README.md) | [Fenwick Tree (Binary Indexed Tree)](../16-fenwick-tree/README.md)
