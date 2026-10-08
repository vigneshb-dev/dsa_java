# Binary Search Tree

> Ordered binary tree where left subtree keys < node key < right subtree keys.

## 1. Overview
A Binary Search Tree (BST) is a node-based binary tree maintaining the BST invariant: every key in the left subtree is strictly smaller than the node key, and every key in the right subtree is strictly greater. This property allows search, insertion, and deletion in time proportional to tree height. An inorder traversal of a valid BST always yields values in strictly ascending sorted order.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Search | O(1) / O(log n) / O(n) | O(h) recursion (O(1) iterative) |
| Insert | O(1) / O(log n) / O(n) | O(h) recursion (O(1) iterative) |
| Delete | O(1) / O(log n) / O(n) | O(h) recursion (O(1) iterative) |
| Inorder Traversal (Sorted) | O(n) / O(n) / O(n) | O(h) |
| Find Min / Max | O(1) / O(log n) / O(n) | O(1) iterative |

Worst-case O(n) occurs when inserting sorted inputs into an un-balanced BST, degenerating into a singly linked list.

## 3. When to Use
- Dynamic datasets requiring both efficient search and maintaining sorted order.
- Finding predecessor, successor, floor, or ceiling of a target value.
- Validating whether an existing binary tree satisfies ordering invariants.
- Inorder traversal needs to produce sorted output without explicit sorting steps.

## 4. When NOT to Use
- Keys are inserted in sorted order without balancing (degrades to O(n) line; use balanced BST like AVL or Red-Black).
- Only exact key lookups are needed without ordering (use `HashMap` for O(1) average time).
- Static datasets where a simple sorted array with binary search suffices without node pointer overhead.

## 5. Why It Works
The invariant `left.val < node.val < right.val` allows binary decision making at each step: if `target < node.val`, eliminate the entire right subtree. This binary elimination reduces the remaining search space by half at each step in balanced trees.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Unordered Search) | Traverse entire tree checking every node | O(n) | O(h) |
| Optimized (BST Directed Search) | Follow left or right child based on BST comparison | O(h) average O(log n) | O(1) iterative |

BST search exploits the ordering invariant to discard one half of the subtree at each node.

## 7. Data Structures Used Here
- `TreeNode`: Standard node with `left` and `right` child pointers.
- `TreeSet<E>` / `TreeMap<K, V>`: Java's standard library self-balancing BST implementation (Red-Black Tree).

## 8. Core Template (Java)
```java
// Iterative BST Search
TreeNode searchBST(TreeNode root, int val) {
    TreeNode curr = root;
    while (curr != null && curr.val != val) {
        if (val < curr.val) {
            curr = curr.left;
        } else {
            curr = curr.right;
        }
    }
    return curr;
}

// Inorder validation skeleton (monotonic check)
boolean isValidBST(TreeNode root, Long min, Long max) {
    if (root == null) return true;
    if (root.val <= min || root.val >= max) return false;
    return isValidBST(root.left, min, (long) root.val) && 
           isValidBST(root.right, (long) root.val, max);
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Binary Tree](../08-binary-tree/README.md) | [Balanced Trees](../10-balanced-trees/README.md)
