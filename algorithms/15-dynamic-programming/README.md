# Dynamic Programming
> Solving optimization problems by breaking them into overlapping subproblems with optimal substructure and caching results.

## 1. Overview
Dynamic Programming (DP) is an algorithmic paradigm that solves complex problems by breaking them down into simpler, overlapping subproblems. By storing the results of subproblems (memoization or tabulation), it ensures each unique subproblem is computed exactly once, turning exponential time complexities into polynomial time.

## 2. Input / Output
- Input: Problem constraints, choices, and state variables (e.g. integer capacity $W$, item values and weights).
- Output: Optimal objective value (min/max), reachability boolean, or distinct ways count (e.g. maximum value `220`).

## 3. Constraints
- State space dimensions dictate feasibility:
  - 1D DP: $N \le 10^7$ ($O(N)$ time).
  - 2D DP: $N, M \le 5000$ ($O(NM)$ time $\le 2.5 \times 10^7$).
  - 3D DP: $N, M, K \le 300$.
  - Bitmask DP: $N \le 20$ ($O(N^2 \cdot 2^N)$).
- Problem must satisfy two core properties: **Overlapping Subproblems** and **Optimal Substructure**.

## 4. Brute-Force Approach
- Idea: Exhaustively evaluate all branches in the decision tree using recursion without caching results.
- Pseudocode:
  ```java
  int solve(int i, int w) {
      if (i == 0 || w == 0) return 0;
      int skip = solve(i - 1, w);
      int take = (wt[i] <= w) ? val[i] + solve(i - 1, w - wt[i]) : 0;
      return Math.max(skip, take);
  }
  ```
- Time: $O(2^N)$ exponential branching; Space: $O(N)$ recursion depth.

## 5. Optimal Approach
- Idea: Define state `dp[i][w]`, specify base cases, establish the transition recurrence, and populate iteratively (bottom-up) or recursively with memoization (top-down).
```java
// Reusable Dynamic Programming Skeleton (Bottom-Up Tabulation Template)
public class DynamicProgrammingTemplate {
    public static int solve(int[] weights, int[] values, int capacity) {
        int n = weights.length;
        // 1. DP Table Definition: dp[w] = max value with capacity w
        int[] dp = new int[capacity + 1];

        // 2. Base Cases: implicitly 0 for capacity 0

        // 3. Iterative State Transitions
        for (int i = 0; i < n; i++) {
            // Reverse loop for 0/1 Knapsack to use previous state in-place
            for (int w = capacity; w >= weights[i]; w--) {
                dp[w] = Math.max(dp[w], values[i] + dp[w - weights[i]]);
            }
        }

        // 4. Return Final State
        return dp[capacity];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Prevents re-computing identical subproblem states millions of times across disparate recursive call branches.
- **Cases skipped:** Once state `(i, w)` is computed, subsequent invocations read the cached value in $O(1)$ without descending.
- **Shortcuts / tricks used:** Memoization lookup or bottom-up topological state ordering; rolling array space optimization reduces 2D table to 1D.
- **Time saved:** $O(2^N) \to O(N \cdot W)$; exponential reduction to polynomial time bounded by total unique states $\times$ transitions.
- **Space effect:** Allocates $O(N \cdot W)$ table, compressible to $O(W)$ using rolling buffer optimization.
- **Trade-off:** Requires memory allocation for state caching and problem must satisfy strict Bellman optimality.

## 6. Core Idea
An optimal global solution is composed of optimal solutions to its constituent subproblems. By establishing a DAG (Directed Acyclic Graph) of state dependencies, solutions can be resolved systematically from smallest base cases to target state.

## 7. Pattern
- Pattern: Bellman's Principle of Optimality / State Machine DP.
- Signals: "Find minimum/maximum cost", "count total distinct ways", "can reach target", "partition into subsets", "best sequence of choices".

## 8. Data Structure Used
- Multi-dimensional primitive arrays `int[]`, `int[][]`, or `int[][][]` for fast cache lookups.
- Integer bitmasks `(1 << n)` for compact subset representation.

## 9. Invariant
At state index $i$, all required predecessor subproblem dependencies have been completely resolved and contain their mathematically optimal values.

## 10. Dry Run
Climbing Stairs ($n = 4$, $dp[i] = dp[i-1] + dp[i-2]$):
| Step $i$ | `dp[i-2]` | `dp[i-1]` | Computation | `dp[i]` |
|---|---|---|---|---|
| Base 1 | - | - | Base case | 1 |
| Base 2 | - | - | Base case | 2 |
| 3 | 1 | 2 | $1 + 2$ | 3 |
| 4 | 2 | 3 | $2 + 3$ | 5 |

## 11. Edge Cases
- Base cases: $n = 0$, capacity $= 0$, empty string matching.
- State initialization: initialize table with $+\infty$ or $-\infty$ when searching for min/max to prevent default 0 values from polluting answers.
- Array index out of bounds: ensure table dimensions match $[N + 1]$ or $[W + 1]$.

## 12. Correctness
Proved by structural induction on state space DAG: Base states are correct by definition. If all states topologically prior to state $S$ are optimal, transition $S = \min_{p} \{ cost(p, S) + dp[p] \}$ considers all legal predecessor paths, ensuring $S$ is optimal.

## 13. Time Complexity
- Best / Average / Worst: $\Theta(\text{Total States} \times \text{Transitions per State})$. For $N$ items and capacity $W$, $O(N \cdot W)$ pseudo-polynomial time.

## 14. Space Complexity
- Auxiliary Space: $O(\text{States})$ for full DP table; optimizable to $O(\text{Previous Row})$ using rolling arrays.

## 15. Can It Be Optimized?
Can be optimized using Space Optimization (rolling array $O(1)$ or $O(W)$), Convex Hull Trick, Knuth's Optimization, or Divide-and-Conquer DP for range problems.

## 16. When Should I Use This Algorithm?
- Problem exhibits overlapping subproblems and optimal substructure.
- Questions asking for maximum profit, minimum cost, or total number of ways.
- String alignment, edit distance, and subsequence pattern matching.
- Selecting items under capacity/weight constraints (Knapsack family).
- Decision sequences where future choices depend on previous state aggregates.

## 17. When Should I NOT Use It?
- Subproblems do not overlap (e.g. Merge sort; use Divide and Conquer).
- Greedy choice property holds provably (use Greedy for $O(n \log n)$ speed and $O(1)$ memory).
- Problem has cyclical dependencies (states depend on future states without DAG ordering; use Shortest Path algorithms).
- State parameters are continuous or unbounded without discrete states.

---

## Sub-Topics in This Section
| Sub-folder | Description |
|---|---|
| [1d](1d/README.md) | Linear state DP tracking 1D prefix states (Fibonacci, Climbing Stairs, House Robber) |
| [2d-grid](2d-grid/README.md) | Grid-based DP over rows and columns (Unique Paths, Minimum Path Sum) |
| [knapsack](knapsack/README.md) | Subset selection with bounded capacity constraints (0/1, Unbounded, Bounded) |
| [subsequences](subsequences/README.md) | String and array sequence alignment and matching (LCS, LIS, Edit Distance) |
| [partition-and-interval](partition-and-interval/README.md) | Range/Interval DP solving optimal sub-segment splits (Matrix Chain Multiplication, Burst Balloons) |
| [bitmask](bitmask/README.md) | Exponential state DP tracking subset membership via integer bits (TSP, Assignment) |
| [tree-dp](tree-dp/README.md) | Subtree state DP over hierarchical trees using post-order DFS (Tree Diameter, House Robber III) |

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic dynamic programming problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [14 - Greedy](../14-greedy/README.md) | [16 - Tree Algorithms](../16-tree-algorithms/README.md)
