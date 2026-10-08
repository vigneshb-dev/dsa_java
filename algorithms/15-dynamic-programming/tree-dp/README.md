# Tree Dynamic Programming

> State propagation on tree structures: subtree aggregations and re-rooting techniques.

## 1. Overview
Tree Dynamic Programming computes DP states over tree nodes by leveraging the tree's natural hierarchical substructure. In bottom-up Tree DP, a post-order DFS computes subproblem answers for all children before aggregating them into parent state `dp[u]`. Advanced problems employ 're-rooting' (two-pass DFS), where an initial pass computes subtree values and a second pass computes answers for every node as root in O(n) total time.

## 2. Time & Space Complexity
| Pattern / Problem | Method | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| House Robber III (Tree) | Post-order DFS returning `[rob, skip]` | O(n) | O(h) call stack |
| Maximum Path Sum in Tree | Bottom-up post-order with global max | O(n) | O(h) call stack |
| Sum of Distances in Tree | 2-Pass Re-rooting DP | O(n) | O(n) |
| Tree Diameter | Post-order maximum depths | O(n) | O(h) call stack |

Because a tree contains no cycles and has exactly n - 1 edges, each edge and node is traversed a constant number of times.

## 3. When to Use
- Optimization or counting problems defined on trees.
- Decisions at a node depend on decisions made in its child subtrees (e.g., Vertex Cover, Independent Set).
- Computing distances or properties when any arbitrary node can act as the root (Re-rooting DP).
- Maximum weight branch paths in hierarchical graphs.

## 4. When NOT to Use
- Graph contains cycles or multiple paths between nodes (use general graph DP or shortest path).
- Edges are directed with cycles.
- Tree undergoes frequent dynamic edge mutations (use Link-Cut Trees or Heavy-Light Decomposition).

## 5. Why It Works
Removing any node or edge in a tree cleanly disconnects it into independent subtrees. Because child subtrees share no cross-edges, their optimal states are strictly mutually independent and can be combined without double-counting.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Root at each node) | Run full DFS from every vertex as root | O(n^2) | O(n) |
| Re-rooting Tree DP | Pass 1: compute subtree metrics; Pass 2: adjust root transitions | O(n) | O(n) |

Re-rooting DP dynamically updates root metrics in O(1) per edge transition, dropping all-node root evaluation from O(n^2) to O(n).

## 7. Data Structures Used Here
- `List<List<Integer>>`: Tree adjacency list.
- `int[] dp` / `int[] count`: Node state arrays for subtree metrics.

## 8. Core Template (Java)
```java
// House Robber III (Post-order Tree DP returning [rob, skip])
int[] robSubtree(TreeNode root) {
    if (root == null) return new int[]{0, 0};
    int[] left = robSubtree(root.left);
    int[] right = robSubtree(root.right);

    // rob this node: cannot rob immediate children
    int rob = root.val + left[1] + right[1];
    // skip this node: can choose best of robbing or skipping children
    int skip = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

    return new int[]{rob, skip};
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [Bitmask Dynamic Programming](../bitmask/README.md) | End
