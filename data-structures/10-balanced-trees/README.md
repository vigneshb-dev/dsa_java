# Balanced Trees
> Self-balancing binary search trees that maintain logarithmic height O(log n) through tree rotations.

## 1. Fundamentals
- What is it? A binary search tree variant that automatically restructures itself via tree rotations upon every insertion and deletion to maintain $O(\log n)$ height.
- What problem does it solve? Eliminates the $O(n)$ worst-case pathological skewing of standard BSTs, guaranteeing strict $O(\log n)$ worst-case bounds for lookups, insertions, and deletions.
- What type of data does it store? Ordered, comparable elements (`Comparable` or `Comparator`).
- Linear or non-linear? Non-linear (hierarchical balanced tree).
- Static or dynamic? Dynamic: dynamically expands and contracts as nodes are allocated, rotated, and unlinked.
- Ordered or unordered? Strictly ordered: in-order traversal yields items in ascending sorted sequence.
- Mutable or immutable? Mutable in Java: nodes and balancing metadata (heights or colors) are updated on mutations.
- How is the data stored internally? As linked tree nodes storing payload data, left and right child pointers, parent pointers, and balance indicators (`height` integer in AVL; `boolean color` in Red-Black).
```text
Tree Balancing Rotation (Right Rotation on Y):
       [ Y ]                     [ X ]
      /     \                   /     \
    [ X ]   [ C ]   ====>     [ A ]   [ Y ]
   /     \                           /     \
 [ A ]   [ B ]                     [ B ]   [ C ]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Perform standard BST insertion to place new leaf node.
  2. Retrace the ancestor path back up toward root, updating height or checking color invariants.
  3. Detect balance violations and execute 1 or 2 local tree rotations (LL, RR, LR, or RL) to restore balance.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Height is strictly bounded to $O(\log n)$; at most 2 rotations needed in Red-Black trees and AVL insertions.

### Delete
- **How it works:**
  1. Remove target node using standard BST deletion logic (re-routing or swapping with in-order successor).
  2. Retrace upward toward root, fixing balance factors or repairing Red-Black double-black violations.
  3. Perform necessary color flips and rotations up to root.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(log n) | O(log n) |

Note: Guaranteed $O(\log n)$ worst-case time; requires at most 3 rotations in Red-Black trees (can propagate up to $O(\log n)$ rotations in AVL).

### Search
- **How it works:**
  1. Start at root node.
  2. Compare search key against current node key; descend left if smaller, right if larger.
  3. Terminate when key is matched or null reference reached.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Because height is strictly guaranteed $h \le 1.44 \log_2 n$ (AVL) or $h \le 2 \log_2(n + 1)$ (Red-Black), search is guaranteed $O(\log n)$.

### Access
- **How it works:**
  1. Access by key is identical to search.
  2. Access by index (rank query) takes $O(\log n)$ if nodes are augmented with subtree sizes.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: Direct constant-time random indexing is not supported.

### Update
- **How it works:**
  1. For value updates (without key change): locate node in $O(\log n)$ and update payload.
  2. For key updates: delete old node and insert new node to rebalance and preserve ordering.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(log n) | O(1) |

Note: In-place key modifications are forbidden as they violate the search tree invariant.

### Traverse
- **How it works:**
  1. Perform recursive or stack-based in-order traversal (Left, Root, Right).
  2. Visits elements in sorted ascending order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(log n) |
| Average | O(n) | O(log n) |
| Worst | O(n) | O(log n) |

Note: Auxiliary call-stack depth is strictly capped at $O(\log n)$ due to balanced height.

### Sort
- **How it works:**
  1. Insert n elements into balanced tree: each takes $O(\log n)$, totaling $O(n \log n)$ guaranteed.
  2. Perform in-order traversal to output sorted array in $O(n)$ time.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Note: Guarantees $O(n \log n)$ time even on pre-sorted or adversarial inputs, unlike unbalanced BST sort.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert | O(1) | O(log n) | O(log n) | O(log n) |
| Delete | O(1) | O(log n) | O(log n) | O(log n) |
| Search | O(1) | O(log n) | O(log n) | O(1) |
| Access | O(1) | O(log n) | O(log n) | O(1) |
| Update | O(1) | O(log n) | O(log n) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(log n) |
| Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** AVL Tree (strictly balanced: $|h_L - h_R| \le 1$). Trade-off: Fastest lookup speed due to shallowest height, but higher rotation frequency during deletions.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Red-Black Tree | Color invariants ensure longest path $\le 2 \times$ shortest | Fewer rotations on inserts/deletes (max 2/3) | Slightly deeper than AVL; marginally slower search | General-purpose standard libraries (`TreeMap`) |
| AVL Tree | Height difference between subtrees bounded to $\le 1$ | Rigidly balanced; faster lookup queries | More rotations during deletions | Read-intensive workloads |
| B-Tree / B+ Tree | Multi-way search tree with high branching factor | Minimizes disk I/O; huge fan-out | Complex splitting/merging algorithms | Databases, file system indexing |
| Splay Tree | Self-adjusting tree via splay rotations to root | O(1) amortized for frequently accessed items | Worst-case O(n) individual operation | Caches, working-set locality |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| AVL Balance Factor | Node tracks explicit `int height` | Faster reads; slower rebalancing writes | 4 bytes per node for height integer |
| Red-Black Colors | Node tracks `boolean color` (RED/BLACK) | Faster writes; at most 2 rotations on insert | 1 bit (often 1 byte) per node for color |
| Left-Leaning Red-Black (LLRB)| Restricts red links to left child only | Simpler implementation with fewer symmetric cases | Same theoretical bounds as standard Red-Black |

- **Java built-in equivalents:**
  - `java.util.TreeMap`: Standard library sorted map implemented using a Red-Black tree.
  - `java.util.TreeSet`: Standard library sorted set backed internally by `TreeMap`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyAVLTree.java` | AVL Self-Balancing Tree | Left/right single and double rotations, insert, delete, and height maintenance |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [09 - Binary Search Tree](../09-binary-search-tree/README.md) | [11 - Heap & Priority Queue](../11-heap-priority-queue/README.md)
