# Tree Algorithms

> Specialized algorithms on trees including Lowest Common Ancestor, Morris traversal, and tree diameter.

## 1. Overview
Tree Algorithms operate on connected acyclic graphs where any two nodes are connected by exactly one simple path. This structural property enables powerful specialized techniques including Lowest Common Ancestor (LCA) binary lifting, Morris in-order traversal (O(1) space), and tree diameter computation. Algorithms typically use bottom-up postorder subtree aggregations or top-down path tracking.

## 2. Time & Space Complexity
| Algorithm / Variant | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| LCA (Simple Binary Tree DFS) | O(n) | O(h) recursion |
| LCA (Binary Lifting Precomputation) | O(n log n) precalc, O(log n) query | O(n log n) |
| Morris Traversal (Inorder / Preorder) | O(n) | O(1) space |
| Tree Diameter | O(n) | O(h) recursion |
| Serialize / Deserialize Tree | O(n) | O(n) |

Morris traversal achieves O(1) auxiliary space by temporarily modifying empty right child pointers of in-order predecessors into threaded back-edges.

## 3. When to Use
- Finding Lowest Common Ancestor (LCA) of two nodes.
- Traversing binary trees with strict O(1) auxiliary memory (Morris traversal).
- Computing diameter, maximum path sum, or longest univalue path.
- Encoding/decoding tree hierarchical structures for transmission or storage.

## 4. When NOT to Use
- Structure contains cycles or multiple paths between nodes (use general graph algorithms).
- Graph is disconnected (forest) without handling roots independently.
- Modifying tree pointers in-place is disallowed in concurrent environments (precludes Morris traversal).

## 5. Why It Works
Because an n-node tree contains exactly n - 1 edges and no cycles, every node decomposes the tree into disjoint subtrees. Post-order traversal computes attributes of child subtrees first, combining them at the parent with zero ambiguity.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Path Trace LCA | Find path from root to p and q; compare lists | O(n) | O(n) auxiliary |
| Single-Pass DFS LCA | Recurse subtrees; return node if p and q found in separate branches | O(n) | O(h) stack |

Single-pass DFS eliminates auxiliary node path lists by leveraging recursion unwinding directly.

## 7. Data Structures Used Here
- `TreeNode`: Standard binary tree node structure.
- `ArrayDeque<TreeNode>`: Queue for BFS serialization or level processing.

## 8. Core Template (Java)
```java
// Lowest Common Ancestor in a Binary Tree (DFS)
TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
    if (root == null || root == p || root == q) {
        return root;
    }
    TreeNode left = lowestCommonAncestor(root.left, p, q);
    TreeNode right = lowestCommonAncestor(root.right, p, q);
    if (left != null && right != null) {
        return root; // p and q are in different subtrees
    }
    return left != null ? left : right;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Dynamic Programming](../15-dynamic-programming/README.md) | [Graph Algorithms](../17-graph-algorithms/README.md)
