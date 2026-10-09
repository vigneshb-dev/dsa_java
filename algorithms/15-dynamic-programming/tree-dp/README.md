# Tree Dynamic Programming
> Subtree state optimization and rerooting techniques executed over hierarchical graphs using post-order DFS.

## 1. Overview
Tree Dynamic Programming solves optimization problems on trees (acyclic connected graphs) by leveraging the natural inductive hierarchy of root and subtrees. Using post-order Depth-First Search (DFS), parent nodes aggregate optimal values computed from their disjoint child subtrees. Common applications include Tree Diameter, Binary Tree Maximum Path Sum, and Maximum Independent Set (House Robber III).

## 2. Input / Output
- Input: Root of a tree or adjacency list of $N$ nodes (e.g. root node with values `[3, 2, 3, null, 3, null, 1]`).
- Output: Optimal subtree value or global tree path metric (e.g. maximum non-adjacent robbery sum `7`).

## 3. Constraints
- Tree node count $N \le 10^5$.
- Operates in $O(N)$ linear time because a tree of $N$ vertices contains exactly $N - 1$ edges.
- Tree must be acyclic.

## 4. Brute-Force Approach
- Idea: For every node $u$, run a separate DFS traversal over the entire tree to compute path or subtree properties.
- Pseudocode: Nested DFS calls evaluating all pairs or paths.
- Time: $O(N^2)$; Space: $O(N)$ recursion depth.

## 5. Optimal Approach
- Idea: Perform a single post-order DFS traversal. Each node computes its state from the returned states of its immediate children in $O(1)$ time, maintaining a global variable or returning a composite state tuple.
```java
// Reusable Tree DP Template (House Robber III / Maximum Independent Set on Tree)
public class TreeDPTemplate {
    static class TreeNode {
        int val;
        TreeNode left, right;
        TreeNode(int val) { this.val = val; }
    }

    // Returns an array of size 2: [maxRobbingThisNode, maxSkippingThisNode]
    public static int[] dfs(TreeNode root) {
        if (root == null) {
            return new int[]{0, 0}; // Base case: null node yields 0 value
        }

        // 1. Post-order DFS: solve left and right child subtrees first
        int[] left = dfs(root.left);
        int[] right = dfs(root.right);

        // 2. State Transitions:
        // Case A: Rob current node -> cannot rob immediate children
        int robCurrent = root.val + left[1] + right[1];

        // Case B: Skip current node -> can either rob or skip each child
        int skipCurrent = Math.max(left[0], left[1]) + Math.max(right[0], right[1]);

        return new int[]{robCurrent, skipCurrent};
    }

    public static int robTree(TreeNode root) {
        int[] result = dfs(root);
        return Math.max(result[0], result[1]);
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Subtree computations are never repeated; each subtree returns its summary metrics to its parent once.
- **Cases skipped:** Internal subtree permutations that do not produce optimal boundaries are pruned before bubbling up.
- **Shortcuts / tricks used:** Returning multiple states simultaneously (e.g. `[take, skip]`) in a small array avoids memoization map lookups.
- **Time saved:** $O(N^2) \to O(N)$; each edge is traversed exactly twice (down and up).
- **Space effect:** Avoids large tables; memory is strictly bounded by call stack depth $O(H)$.
- **Trade-off:** Deep skewed trees can cause recursion stack overflow (up to $O(N)$ depth).

## 6. Core Idea
A tree is an acyclic graph where removing a node disconnects the graph into completely independent subtrees. Post-order traversal guarantees that when processing node $u$, all subproblems for its subtrees have already been completely solved.

## 7. Pattern
- Pattern: Post-Order Subtree Aggregation / Tree Rerooting DP.
- Signals: "Binary tree maximum path sum", "house robber on binary tree", "diameter of tree", "sum of distances in tree (rerooting)", "minimum height trees".

## 8. Data Structure Used
- State tuples (e.g. `int[2]`) returned by recursive call frames.
- Recursion call stack (depth $O(H)$).

## 9. Invariant
When DFS unwinds from child $v$ to parent $u$, the returned tuple accurately summarizes the optimal subproblem values for the entire subtree rooted at $v$.

## 10. Dry Run
Subtree aggregation on `[3, (left: 2, leaf), (right: 3, leaf)]`:
| Node | Subtree Left `[rob, skip]` | Subtree Right `[rob, skip]` | `robCurrent` | `skipCurrent` | Returned Tuple |
|---|---|---|---|---|---|
| Leaf 2 | `[0, 0]` | `[0, 0]` | $2 + 0 + 0 = 2$ | $\max(0,0) + \max(0,0) = 0$ | `[2, 0]` |
| Leaf 3 | `[0, 0]` | `[0, 0]` | $3 + 0 + 0 = 3$ | $0$ | `[3, 0]` |
| Root 3 | `[2, 0]` | `[3, 0]` | $3 + 0 + 0 = 3$ | $\max(2,0) + \max(3,0) = 5$ | `[3, 5]` |

Optimal choice: $\max(3, 5) = 5$.

## 11. Edge Cases
- `root == null`: base case returning zeroes.
- Tree with all negative values: initialize global path sum tracker to `Integer.MIN_VALUE`, not 0.
- Skewed tree with depth $10^5$: requires iterative post-order or enlarged stack size.

## 12. Correctness
Proved by structural induction on tree depth: Base leaves return trivial values. If children $L$ and $R$ return correct subproblem solutions, then because $L$ and $R$ share no nodes other than parent $P$, the independence of subproblems holds, ensuring combination at $P$ is globally optimal for that subtree.

## 13. Time Complexity
- Best / Average / Worst: $O(N)$ linear time. Every node is visited once during the post-order DFS.

## 14. Space Complexity
- Auxiliary Space: $O(H)$ where $H$ is the height of the tree ($O(\log N)$ balanced, $O(N)$ skewed).

## 15. Can It Be Optimized?
Linear $O(N)$ time is optimal. For problems requiring answers for EVERY node as the root (e.g. Sum of Distances in Tree), **Tree Rerooting DP** computes all $N$ answers in $O(N)$ total time using two DFS passes instead of $O(N^2)$.

## 16. When Should I Use This Algorithm?
- Optimization problems defined on trees or hierarchical graphs.
- Tree Diameter and longest path between any two nodes.
- Maximum Independent Set on trees.
- Binary Tree Maximum Path Sum.
- All-node distance aggregations (Tree Rerooting).

## 17. When Should I NOT Use It?
- General graphs containing cycles (cycles create circular dependencies; use Bellman-Ford or Dijkstra).
- Trees with dynamic edge insertions/deletions during queries (use Link-Cut Trees or Heavy-Light Decomposition).
- Simple tree reachability without optimization (use standard BFS/DFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic tree DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Bitmask DP](../bitmask/README.md) | [Tree Algorithms](../../16-tree-algorithms/README.md)
