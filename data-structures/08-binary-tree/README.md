# Binary Tree

> Hierarchical node structure where each node has at most two children (left and right).

## 1. Overview
A Binary Tree is a non-linear hierarchical data structure in which each node has a value and at most two child pointers: left and right. Unlike arrays or lists, trees naturally represent hierarchical relationships and divide-and-conquer subproblem decompositions. Binary trees are traversed using Depth-First Search (preorder, inorder, postorder) or Breadth-First Search (level order).

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Traversal (DFS / BFS) | O(n) / O(n) / O(n) | O(h) recursion / O(w) queue |
| Find Height / Max Depth | O(n) / O(n) / O(n) | O(h) |
| Check Symmetry / Identity | O(n) / O(n) / O(n) | O(h) |
| Lowest Common Ancestor | O(n) / O(n) / O(n) | O(h) |

Height `h` ranges from O(log n) in balanced binary trees to O(n) in degenerate skewed trees; `w` is maximum width (up to n/2 at bottom level).

## 3. When to Use
- Hierarchical relationships such as file systems, expression trees, or decision trees.
- Problems asking for paths, maximum depth, subtree sums, or diameter.
- Divide-and-conquer processing where subtrees are solved independently and merged.
- Level-by-level inspection or serialization/deserialization tasks.

## 4. When NOT to Use
- Data is strictly linear and has no hierarchical structure (use array or list).
- Sorted key search in O(log n) is needed without balancing guarantees (use balanced BST).
- Nodes have arbitrary or dynamic numbers of children (use N-ary general tree representation).

## 5. Why It Works
Binary tree recursive solutions work because every subtree is itself a complete binary tree. The base case handles empty leaf children (`null`), and the recursive step combines solutions from the left and right subtrees.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Diameter by Height) | Compute height at every node separately | O(n^2) | O(h) |
| Optimized (Bottom-Up Postorder) | Return height while updating diameter globally in one pass | O(n) | O(h) |

The bottom-up postorder pass computes height and diameter simultaneously, avoiding repeated subtree traversals.

## 7. Data Structures Used Here
- `TreeNode`: Standard node definition containing `int val`, `TreeNode left`, `TreeNode right`.
- `ArrayDeque<TreeNode>`: Queue used for BFS level-order traversal.

## 8. Core Template (Java)
```java
// Standard TreeNode class definition
class TreeNode {
    int val;
    TreeNode left, right;
    TreeNode(int val) { this.val = val; }
}

// DFS recursion skeleton
int dfs(TreeNode root) {
    if (root == null) return 0;
    int left = dfs(root.left);
    int right = dfs(root.right);
    return Math.max(left, right) + 1;
}

// BFS Level Order skeleton
void bfs(TreeNode root) {
    if (root == null) return;
    Queue<TreeNode> queue = new ArrayDeque<>();
    queue.offer(root);
    while (!queue.isEmpty()) {
        int levelSize = queue.size();
        for (int i = 0; i < levelSize; i++) {
            TreeNode node = queue.poll();
            if (node.left != null) queue.offer(node.left);
            if (node.right != null) queue.offer(node.right);
        }
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Hashing](../07-hashing/README.md) | [Binary Search Tree](../09-binary-search-tree/README.md)
