# Binary Search Tree
> Node-based binary tree data structure where left descendants are strictly smaller and right descendants are strictly larger than the root.

## 1. Fundamentals
- What is it? A binary tree enforcing the binary search invariant: for any node, all keys in its left subtree are smaller, and all keys in its right subtree are larger.
- What problem does it solve? Provides dynamic sorted-order maintenance with average O(log n) lookups, insertions, deletions, predecessor, and successor queries.
- What type of data does it store? Comparable keys and associated values (`Comparable<T>` or `Comparator<T>`).
- Linear or non-linear? Non-linear (hierarchical search tree).
- Static or dynamic? Dynamic: individual nodes are allocated on the heap and linked via references.
- Ordered or unordered? Strictly ordered: an in-order traversal visits elements in ascending sorted order.
- Mutable or immutable? Mutable in Java: nodes and subtree branches can be inserted, deleted, and rotated.
- How is the data stored internally? As linked `BSTNode` objects containing `key`, `val`, `left`, and `right` references on the heap.
```text
Binary Search Tree Memory Layout:
             [ Key: 8 ]
             /        \
      [ Key: 3 ]     [ Key: 10 ]
      /        \               \
  [ Key: 1 ]  [ Key: 6 ]     [ Key: 14 ]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Compare target key against current node key.
  2. If target is smaller, descend left; if larger, descend right.
  3. When a `null` reference is encountered, allocate a new `BSTNode` and attach it.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(n) | O(n) |

Note: Worst-case O(n) time and call-stack space occurs when keys are inserted in sorted order, forming a skewed linear chain.

### Delete
- **How it works:**
  1. Search for target node. If node has 0 children (leaf), sever reference.
  2. If node has 1 child, bypass target by connecting parent directly to that child.
  3. If node has 2 children, find in-order successor (minimum in right subtree), copy its value to target, and delete successor.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(log n) |
| Worst | O(n) | O(n) |

Note: In-order successor discovery and deletion take O(h) where h is tree height.

### Search
- **How it works:**
  1. Start at root. If key equals current, return node.
  2. If key < current key, descend to left child; if key > current key, descend to right child.
  3. Return null if a null reference is reached without finding the key.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(n) | O(1) |

Note: Iterative search takes O(1) auxiliary space; recursive search takes O(h) call-stack frames.

### Access
- **How it works:**
  1. Standard BST access by key is identical to search.
  2. Positional k-th element access requires traversing in-order (O(k)) or augmenting nodes with subtree sizes (Order-Statistic Tree for O(log n)).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(n) | O(1) |

Note: Direct 0-based array index access is not natively supported without subtree size metadata.

### Update
- **How it works:**
  1. If updating an associated value for an existing key: locate key in O(h) and overwrite `node.val = newVal`.
  2. If changing the key itself: delete old key and re-insert new key to preserve the BST invariant.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(log n) | O(1) |
| Worst | O(n) | O(1) |

Note: In-place key mutation is forbidden as it breaks the BST ordering property.

### Traverse
- **How it works:**
  1. In-order traversal: recursively visit left subtree, process current node, visit right subtree.
  2. Guarantees sorted non-decreasing output order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(h) |
| Average | O(n) | O(log n) |
| Worst | O(n) | O(n) |

Note: Iterative traversal using Morris Traversal achieves O(n) time in O(1) auxiliary space.

### Sort
- **How it works:**
  1. Tree Sort: Insert all n elements into the BST sequentially.
  2. Perform an in-order traversal to collect elements back into an array in sorted order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n^2) | O(n) |

Note: Worst-case O(n^2) time occurs when inserting already-sorted inputs into an unbalancing BST.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert | O(1) | O(log n) | O(n) | O(h) |
| Delete | O(1) | O(log n) | O(n) | O(h) |
| Search | O(1) | O(log n) | O(n) | O(1) |
| Access (by Key) | O(1) | O(log n) | O(n) | O(1) |
| Update (Value) | O(1) | O(log n) | O(n) | O(1) |
| Traverse (In-Order) | O(n) | O(n) | O(n) | O(h) |
| Sort (Tree Sort) | O(n log n) | O(n log n) | O(n^2) | O(n) |

## 3. Variations
- **Standard version:** Vanilla Unbalanced BST. Trade-off: Straightforward implementation, but degenerates into an O(n) linked list under sorted or pathological insertion order.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| AVL Tree | Enforces strict height balance condition: $|h_L - h_R| \le 1$ | Strict $O(\log n)$ worst-case bounds; faster lookups | Slower insertions/deletions due to frequent rotations | Read-heavy sorted dictionary lookups |
| Red-Black Tree | Enforces color invariants ensuring longest path $\le 2 \times$ shortest | Relaxed balance; fewer rotations on write operations | Slightly taller than AVL (slower search by constant factor) | General-purpose libraries (`TreeMap`, Linux CFS) |
| Splay Tree | Splays accessed node to root via tree rotations | Frequently accessed nodes become O(1); no balance metadata | Worst-case O(n) single operation; cache writes on reads | Caching, paging, memory allocators |
| Treap | Randomly assigns heap priorities to maintain heap-ordered BST | Randomized balance with simple rotations; no complex color logic | Requires random number generation; priority overhead | Randomized sets, split/merge rope trees |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Recursive BST | Uses call stack for tree traversal and path reconstruction | Clean code; O(h) stack memory overhead | High risk of `StackOverflowError` on skewed trees |
| Iterative BST | Uses manual loops and explicit pointer tracking | Avoids call stack overflow; slightly faster execution | O(1) auxiliary space for search; slightly more verbose |
| Parent-Pointer BST | Each node stores a pointer to its immediate parent node | Enables O(1) successor/predecessor step without stack | 8 additional bytes per node reference |

- **Java built-in equivalents:**
  - `java.util.TreeMap`: Red-Black tree implementation of `NavigableMap`.
  - `java.util.TreeSet`: Red-Black tree implementation of `NavigableSet` backed internally by `TreeMap`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyBinarySearchTree.java` | Standard BST | Recursive and iterative search, insert, 3-case delete, and in-order traversal |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [08 - Binary Tree](../08-binary-tree/README.md) | [10 - Balanced Trees](../10-balanced-trees/README.md)
