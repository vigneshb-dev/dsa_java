# 2D Grid Dynamic Programming
> Path finding, boundary traversal, and subgrid optimization across 2D coordinate matrices.

## 1. Overview
2D Grid Dynamic Programming models optimization problems on an $M \times N$ matrix where transitions are constrained to directional moves (typically down and right). It solves problems like Unique Paths, Minimum Path Sum, Dungeon Game, and Maximal Square by expressing cell $(r, c)$ in terms of adjacent cells.

## 2. Input / Output
- Input: An $M \times N$ grid of values or obstacles (e.g. `grid = [[1, 3, 1], [1, 5, 1], [4, 2, 1]]`).
- Output: Minimum path sum or unique path count (e.g. minimum sum `7` via `1 -> 3 -> 1 -> 1 -> 1`).

## 3. Constraints
- $M, N \le 1,000 \implies M \times N \le 10^6$ total states, running in under 0.1s.
- Moves are directional without cycles (directed acyclic grid graph).

## 4. Brute-Force Approach
- Idea: Recursive exploration of all possible paths from top-left to bottom-right.
- Pseudocode: `int path(r, c) = grid[r][c] + Math.min(path(r+1, c), path(r, c+1));`
- Time: $O(2^{M+N})$; Space: $O(M + N)$ call stack.

## 5. Optimal Approach
- Idea: Compute states row by row. Notice `dp[r][c]` depends only on `dp[r-1][c]` (above) and `dp[r][c-1]` (left), allowing space optimization to a single row array `dp[c]`.
```java
// Reusable 2D Grid DP Template (Minimum Path Sum, Space-Optimized to O(N))
import java.util.Arrays;

public class TwoD_GridDPTemplate {
    public static int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[] dp = new int[n];

        dp[0] = grid[0][0];
        // Initialize top row prefix sums
        for (int c = 1; c < n; c++) dp[c] = dp[c - 1] + grid[0][c];

        for (int r = 1; r < m; r++) {
            dp[0] += grid[r][0]; // First column can only come from above
            for (int c = 1; c < n; c++) {
                // dp[c] currently holds value from above (row r - 1);
                // dp[c - 1] holds newly updated value from left (row r)
                dp[c] = grid[r][c] + Math.min(dp[c], dp[c - 1]);
            }
        }

        return dp[n - 1];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Prevents re-exploring overlapping paths converging at the same `(r, c)` coordinate.
- **Cases skipped:** Suboptimal path prefixes reaching `(r, c)` are discarded by the `Math.min` operation.
- **Shortcuts / tricks used:** 1D rolling array replaces the $M \times N$ matrix by overwriting previous row entries in-place.
- **Time saved:** $O(2^{M+N}) \to O(M \cdot N)$.
- **Space effect:** $O(M \cdot N) \to O(N)$ auxiliary space.
- **Trade-off:** In-place 1D buffer cannot trace back the full optimal path coordinate list without retaining directional pointers.

## 6. Core Idea
Because valid motion is restricted to down and right, cell $(r, c)$ can only be reached from $(r-1, c)$ or $(r, c-1)$. Solving subproblems in row-major order guarantees dependencies are always resolved prior to visiting each cell.

## 7. Pattern
- Pattern: Directed Acyclic Grid Traversal.
- Signals: "Robot starts at top-left, moves only down or right", "minimum path sum in grid", "unique paths with obstacles", "cherry pickup", "maximal square of 1s".

## 8. Data Structure Used
- 1D primitive array `int[N]` (or 2D table `int[M][N]`).

## 9. Invariant
At step $(r, c)$, `dp[c]` stores the minimum path sum from $(0, 0)$ to cell $(r, c)$.

## 10. Dry Run
Grid `[[1, 3], [1, 5]]`:
| Row $r$ | Col $c$ | Cell Value | Above (`dp[c]`) | Left (`dp[c-1]`) | Updated `dp[c]` |
|---|---|---|---|---|---|
| 0 | 0 | 1 | - | - | 1 |
| 0 | 1 | 3 | - | 1 | $1 + 3 = 4$ |
| 1 | 0 | 1 | 1 | - | $1 + 1 = 2$ |
| 1 | 1 | 5 | 4 | 2 | $5 + \min(4, 2) = 7$ |

Final result: `dp[1] = 7`.

## 11. Edge Cases
- Single row ($1 \times N$) or single column ($M \times 1$): paths are unique and deterministic.
- Obstacles at starting cell $(0, 0)$ or target cell $(M-1, N-1)$: immediately render paths impossible.
- Integer overflow on path counts: use `int` with modulo or `long`.

## 12. Correctness
By induction on Manhattan distance $r + c$: Any path to $(r, c)$ must pass through either $(r-1, c)$ or $(r, c-1)$ as its penultimate step. Since both predecessor cells have strictly smaller Manhattan distances, their optimal values are known, proving $\min(dp[r-1][c], dp[r][c-1]) + cost(r, c)$ is optimal.

## 13. Time Complexity
- Best / Average / Worst: $O(M \cdot N)$ iterating through all grid cells exactly once.

## 14. Space Complexity
- Auxiliary Space: $O(N)$ using rolling row buffer (or $O(M \cdot N)$ for full table).

## 15. Can It Be Optimized?
For unweighted Unique Paths without obstacles, combinatorics gives an exact $O(\min(M, N))$ answer via binomial coefficients: $\binom{M+N-2}{M-1}$.

## 16. When Should I Use This Algorithm?
- Moving in grid with monotonic direction constraints (e.g. right and down).
- Finding shortest or cheapest path across a cost grid.
- Counting unique paths with or without obstacles.
- Finding largest square or rectangle of 1s in a binary matrix.

## 17. When Should I NOT Use It?
- Movement is allowed in all 4 directions (up, down, left, right) with non-negative weights (creates cycles; use Dijkstra's Algorithm or BFS).
- Negative weight cycles exist (use Bellman-Ford).
- Unconstrained graph topology (use Graph DP).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic 2D grid DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [1D DP](../1d/README.md) | [Knapsack DP](../knapsack/README.md)
