# Binary Tree
> Hierarchical non-linear data structure where each node has at most two children, referred to as left and right.

## 1. Fundamentals
- What is it? A tree data structure in which every node contains a value and references to at most two child nodes (left and right).
- What problem does it solve? Models hierarchical relationships, expression syntax trees (ASTs), file system directories, and serves as the foundation for search trees and heaps.
- What type of data does it store? Generic payload elements wrapped inside tree node objects.
- Linear or non-linear? Non-linear (hierarchical).
- Static or dynamic? Dynamic: nodes are allocated individually on the heap and linked via reference pointers.
- Ordered or unordered? Unordered by element values (unlike BST); structurally ordered (left child vs right child distinctions).
- Mutable or immutable? Mutable in Java: node payloads and child pointers can be modified.
- How is the data stored internally? As linked `TreeNode` objects containing `val`, `TreeNode left`, and `TreeNode right` pointers, or sequentially in an array for complete trees (`left = 2i + 1`, `right = 2i + 2`).
```text
Binary Tree Internal Node Layout:
             [ Root: 1 ]
             /         \
       [ Left: 2 ]   [ Right: 3 ]
       /         \
  [ Leaf: 4 ]  [ Leaf: 5 ]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. For a general binary tree, insertions typically fill the first available spot in level-order (BFS).
  2. Enqueue root in a queue; dequeue nodes until a node with a missing left or right child is encountered.
  3. Attach new node to the vacant reference and exit.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Attaching to an already known parent node is O(1); finding the first available slot via BFS requires O(n) time and O(w) queue memory.

### Delete
- **How it works:**
  1. Locate target node to delete using BFS traversal.
  2. Continue BFS to find the deepest and rightmost node in the tree.
  3. Copy deepest node's value into the target node, then sever and nullify the pointer to the deepest node.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Preserves tree shape without leaving internal pointer holes; requires full traversal (O(n)).

### Search
- **How it works:**
  1. Because values are unsorted, inspect nodes recursively via DFS or iteratively via BFS.
  2. Compare each node's value against target.
  3. Return node reference if found; return null if entire tree is traversed without a match.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(h) |
| Worst | O(n) | O(n) |

Note: Worst-case space is O(n) for skewed degenerate trees; O(log n) auxiliary space for balanced trees.

### Access
- **How it works:**
  1. Binary trees do not support constant-time random access by index.
  2. Accessing a node requires following a traversal path from root (e.g. left-left-right) or searching sequentially.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(h) |
| Worst | O(n) | O(n) |

Note: Direct access by index is N/A; path-based access takes O(depth) = O(h).

### Update
- **How it works:**
  1. Search for the target node using DFS or BFS.
  2. Once reference is obtained, assign `node.val = newVal`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(h) |
| Worst | O(n) | O(n) |

Note: Updating a known node reference is O(1); locating the node requires O(n).

### Traverse
- **How it works:**
  1. Depth-First Traversals: Pre-order (N-L-R), In-order (L-N-R), Post-order (L-R-N) via recursion or explicit stack.
  2. Breadth-First Traversal: Level-order traversal using a FIFO queue.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(h) |
| Average | O(n) | O(h) |
| Worst | O(n) | O(n) |

Note: Visits every node exactly once; space depends on tree height h (O(log n) balanced, O(n) skewed). Morris traversal can achieve O(1) auxiliary space.

### Sort
- **How it works:**
  1. General binary trees do not maintain sorted order.
  2. Sorting requires extracting elements into an array and sorting in O(n log n), or reorganizing nodes into a Binary Search Tree (BST).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Note: In-place sorting of an arbitrary general binary tree is not natively supported.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Level-Order) | O(1) | O(n) | O(n) | O(w) |
| Delete (Deepest Swap)| O(1) | O(n) | O(n) | O(w) |
| Search | O(1) | O(n) | O(n) | O(h) |
| Access (by Path) | O(1) | O(h) | O(n) | O(1) |
| Update | O(1) | O(n) | O(n) | O(h) |
| Traverse | O(n) | O(n) | O(n) | O(h) |
| Sort (Extract & Sort)| O(n log n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** General Pointer-Linked Binary Tree. Trade-off: High structural flexibility for arbitrary branching, but high memory overhead per node (two pointers) and O(n) unsorted search time.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Full Binary Tree | Every node has either 0 or 2 children | Compact mathematical properties; efficient encoding | Restricts node configurations | Huffman coding trees |
| Complete Binary Tree | All levels filled except possibly the last (filled left-to-right) | Can be packed cleanly into a contiguous 1D array | Requires strict insertion order | Binary heaps, priority queues |
| Perfect Binary Tree | All interior nodes have 2 children; all leaves at identical depth | Exactly $2^{h+1}-1$ nodes; perfectly balanced | Extremely rigid size requirements | Theoretical analysis, full tournament trees |
| Threaded Binary Tree | Null child pointers store in-order predecessor/successor links | In-order traversal without recursion or stack in O(1) space | Complex pointer maintenance during updates | Memory-constrained systems |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Pointer-Linked (`TreeNode`) | Explicit heap node objects holding `left` and `right` | Fast local updates; high cache miss rate | 24-32 bytes per node object |
| Array-Backed Complete Tree | Node at `i` has left child `2i+1`, right child `2i+2` | Optimal cache locality; zero pointer overhead | Wasted memory if tree is sparse or unbalanced |

- **Java built-in equivalents:**
  - Standard Java has no standalone public general `BinaryTree` class in `java.util`. Complete binary trees are used internally in `java.util.PriorityQueue` (array-backed), and red-black binary trees are used in `TreeMap` and `HashMap`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyBinaryTree.java` | Pointer-based Binary Tree | Node structure with DFS traversals, BFS level-order insertion, and height calculation |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [07 - Hashing](../07-hashing/README.md) | [09 - Binary Search Tree](../09-binary-search-tree/README.md)
