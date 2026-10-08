# 2D Grid Dynamic Programming

> State transitions on coordinate grids where `dp[i][j]` depends on adjacent top/left cells.

## 1. Overview
2D Grid DP evaluates optimization problems defined over a matrix or coordinate plane. The state `dp[r][c]` typically aggregates outcomes from the left cell `dp[r][c-1]` and top cell `dp[r-1][c]`. Prominent examples include Unique Paths, Minimum Path Sum, and Dungeon Game.

## 2. Time & Space Complexity
| Problem / Variant | Transition | Time | Space (Standard / Optimized) |
| :--- | :--- | :--- | :--- |
| Unique Paths | `dp[r][c] = dp[r-1][c] + dp[r][c-1]` | O(m * n) | O(m * n) / O(n) |
| Minimum Path Sum | `dp[r][c] = min(dp[r-1][c], dp[r][c-1]) + cost` | O(m * n) | O(m * n) / O(n) |
| Maximal Square | `dp[r][c] = min(top, left, diag) + 1` | O(m * n) | O(m * n) / O(n) |
| Dungeon Game | Reverse DP from `(m-1, n-1)` to `(0, 0)` | O(m * n) | O(m * n) / O(n) |

Because row `r` depends exclusively on row `r - 1` and the current row, space can be compressed to a single 1D array of size `n`.

## 3. When to Use
- Navigation on an m x n grid with restricted movement directions (e.g., only right and down).
- Finding the path with minimal or maximal accumulated cell costs.
- Finding maximal square or rectangular submatrices satisfying specific criteria.
- Obstacles or hazards present on grid cells.

## 4. When NOT to Use
- Movement is permitted in all 4 or 8 directions (causes dependency cycles; use BFS or Dijkstra).
- Grid has weighted edge costs with unrestricted traversal cycles.
- Grid dimensions are excessively large (e.g., m, n >= 10^5).

## 5. Why It Works
When movement is restricted to right and down, the grid constitutes a Directed Acyclic Graph. Row-by-row or column-by-column iteration evaluates states in exact topological order.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Recursive Path Search | Try all right and down branches recursively | O(2^(m+n)) | O(m + n) stack |
| 2D DP Tabulation | Fill grid row by row summing incoming path values | O(m * n) | O(n) space |

Tabulating across the grid converts an exponential combinatorial path count into polynomial cell iterations.

## 7. Data Structures Used Here
- `int[][]`: Full 2D tabulation table.
- `int[]`: Single rolling row array for space optimization.

## 8. Core Template (Java)
```java
// Minimum Path Sum with 1D rolling array optimization
int minPathSum(int[][] grid) {
    int m = grid.length, n = grid[0].length;
    int[] dp = new int[n];
    dp[0] = grid[0][0];
    for (int c = 1; c < n; c++) dp[c] = dp[c - 1] + grid[0][c];
    for (int r = 1; r < m; r++) {
        dp[0] += grid[r][0];
        for (int c = 1; c < n; c++) {
            dp[c] = Math.min(dp[c], dp[c - 1]) + grid[r][c];
        }
    }
    return dp[n - 1];
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [1D Dynamic Programming](../1d/README.md) | [Knapsack DP](../knapsack/README.md)
