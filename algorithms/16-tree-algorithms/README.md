# Tree Algorithms
> Structural traversals, lowest common ancestor queries, tree serialization, and boundary transformations over acyclic connected graphs.

## 1. Overview
Tree Algorithms address fundamental topological queries and transformations on tree structures. They encompass Lowest Common Ancestor (LCA), iterative Morris Traversal (in $O(1)$ space), serialization/deserialization, tree diameter, and path constructions, leveraging the unique property that any two nodes in a tree are connected by exactly one simple path.

## 2. Input / Output
- Input: Root node of a tree and query nodes $p, q$ (e.g. root with target nodes `p = 5, q = 1`).
- Output: The lowest common ancestor node (e.g. Node `3`).

## 3. Constraints
- Number of nodes $N \le 10^5$.
- Node values can be duplicate or unique depending on problem specifications.
- Exactly $N - 1$ edges connecting $N$ nodes without cycles.

## 4. Brute-Force Approach
- Idea: Find path from root to node $p$ and path from root to node $q$ in separate traversals, then compare the two paths node-by-node from root until they diverge.
- Pseudocode:
  ```java
  List<TreeNode> pathP = new ArrayList<>(), pathQ = new ArrayList<>();
  findPath(root, p, pathP); findPath(root, q, pathQ);
  // Compare paths
  ```
- Time: $O(N)$ with two passes and extra path storage; Space: $O(N)$ auxiliary path memory.

## 5. Optimal Approach
- Idea: Single-pass recursive post-order DFS. If the current node matches $p$ or $q$, return it. Recurse on left and right subtrees: if both return non-null, current node is the LCA; otherwise propagate the non-null child.
```java
// Reusable Tree Algorithm Skeleton (Lowest Common Ancestor Template)
public class TreeAlgorithmsTemplate {
    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    public static TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // Base case: null root or found target node
        if (root == null || root == p || root == q) {
            return root;
        }

        // Search in left and right subtrees
        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // If both subtrees return non-null, p and q lie in different branches; root is LCA
        if (left != null && right != null) {
            return root;
        }

        // Otherwise return the non-null candidate (either found node or confirmed LCA)
        return left != null ? left : right;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the need to allocate, build, and store explicit path lists from root to target nodes.
- **Cases skipped:** Subtrees that do not contain $p$ or $q$ return `null` immediately and are pruned from consideration.
- **Shortcuts / tricks used:** Bubble-up signaling: passing references up the call stack naturally converges at their lowest common branch point.
- **Time saved:** Retains linear $O(N)$ execution in a single unified pass.
- **Space effect:** Reduces auxiliary space to call stack depth $O(H)$ without $O(N)$ path lists.
- **Trade-off:** Skewed trees require $O(N)$ stack frames.

## 6. Core Idea
Because a tree has no cycles, if $p$ is located in the left subtree of node $u$ and $q$ is located in the right subtree of $u$, then $u$ is mathematically guaranteed to be the lowest common ancestor of $p$ and $q$.

## 7. Pattern
- Pattern: Bottom-Up Tree DFS / Lowest Common Ancestor.
- Signals: "Lowest Common Ancestor (LCA)", "diameter of binary tree", "serialize and deserialize binary tree", "flatten binary tree to linked list", "symmetric tree", "count complete tree nodes".

## 8. Data Structure Used
- Recursion call stack (depth $O(H)$).
- FIFO Queue for level-order (BFS) traversals.

## 9. Invariant
At any node $u$, the recursive call returns non-null if and only if the subtree rooted at $u$ contains at least one of the query nodes (or their resolved LCA).

## 10. Dry Run
LCA of 5 and 1 in `[3, (left: 5), (right: 1)]`:
| Call | `root` | `left` returned | `right` returned | Action | Return Value |
|---|---|---|---|---|---|
| Frame 2 | Node 5 | - | - | Matches $p$ | Returns Node 5 |
| Frame 3 | Node 1 | - | - | Matches $q$ | Returns Node 1 |
| Frame 1 | Node 3 | Node 5 | Node 1 | Both non-null | **Returns Node 3 (LCA)** |

## 11. Edge Cases
- One node is ancestor of the other ($p$ is parent of $q$): correctly returns $p$ immediately without visiting $q$.
- Skewed line tree: recursion depth reaches $N$.
- Duplicate values: match by object reference (`root == p`) rather than primitive value equality.

## 12. Correctness
By structural induction: Base case returns $p$ or $q$ when found. At any node $u$, if both children report hits, $p$ and $q$ reside in disjoint subtrees of $u$, making $u$ the deepest split point (LCA). If only one child reports hits, both targets must reside under that child branch.

## 13. Time Complexity
- Best / Average / Worst: $O(N)$ linear time; visits each node at most once.

## 14. Space Complexity
- Auxiliary Space: $O(H)$ recursion stack space ($O(\log N)$ balanced, $O(N)$ skewed).

## 15. Can It Be Optimized?
For multiple LCA queries ($Q$ queries on a static tree), **Binary Lifting** preprocesses ancestor tables in $O(N \log N)$ time and answers each query in $O(\log N)$. Tarjan's Offline LCA uses Union-Find to answer $Q$ queries in $O(N + Q)$ total time. Morris Traversal performs tree traversals in $O(1)$ auxiliary space.

## 16. When Should I Use This Algorithm?
- Finding Lowest Common Ancestor of two nodes in binary or n-ary trees.
- Serializing and deserializing tree hierarchical structures.
- Checking structural symmetry, isomorphism, or subtree equivalence.
- Computing distances between any two arbitrary nodes in a tree ($\text{dist}(u, v) = \text{depth}(u) + \text{depth}(v) - 2 \cdot \text{depth}(\text{LCA})$).
- Flattening trees into singly linked lists in-place.

## 17. When Should I NOT Use It?
- General graphs containing cycles (use Graph BFS/DFS).
- Frequent dynamic changes to tree topology during queries (use Link-Cut Trees).
- Binary Search Trees where key values can guide the search (use BST LCA in $O(H)$ time without full tree inspection).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic tree algorithm problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [15 - Dynamic Programming](../15-dynamic-programming/README.md) | [17 - Graph Algorithms](../17-graph-algorithms/README.md)
