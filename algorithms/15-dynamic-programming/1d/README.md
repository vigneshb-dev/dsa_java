# 1D Dynamic Programming
> Linear state transitions where subproblem solutions depend on a constant window of prior steps.

## 1. Overview
1D Dynamic Programming resolves optimization problems where the optimal solution at step $i$ depends strictly on a fixed number of preceding states ($i-1, i-2, \dots$). It models problems like Climbing Stairs, House Robber, Fibonacci numbers, and Decode Ways, often admitting space reduction to $O(1)$.

## 2. Input / Output
- Input: An array or single integer $n$ (e.g. `nums = [2, 7, 9, 3, 1]`).
- Output: Optimal maximum, minimum, or count (e.g. maximum robbery amount `12`).

## 3. Constraints
- $N \le 10^7$ because transition cost is $O(1)$ per state, running in linear time.
- State at $i$ depends only on a local constant number of prior steps.

## 4. Brute-Force Approach
- Idea: Explore all binary choices (take vs skip) recursively at each step.
- Pseudocode: `int rob(i) = Math.max(rob(i-1), nums[i] + rob(i-2));`
- Time: $O(2^n)$; Space: $O(n)$ recursion call stack.

## 5. Optimal Approach
- Idea: Maintain state variables `prev2` and `prev1` tracking optimal solutions for $i-2$ and $i-1$, updating iteratively in $O(1)$ memory.
```java
// Reusable 1D DP Space-Optimized Template (House Robber)
public class OneD_DPTemplate {
    public static int rob(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
        if (nums.length == 1) return nums[0];

        int prev2 = 0;       // dp[i - 2]
        int prev1 = nums[0]; // dp[i - 1]

        for (int i = 1; i < nums.length; i++) {
            int take = nums[i] + prev2;
            int skip = prev1;
            int current = Math.max(take, skip);

            prev2 = prev1;
            prev1 = current;
        }

        return prev1;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes exponential redundant recursion trees evaluating identical sub-indices.
- **Cases skipped:** Branches with suboptimal choices at previous steps are pruned immediately by the `Math.max` state collapse.
- **Shortcuts / tricks used:** Storing only `prev1` and `prev2` replaces the full $O(n)$ table.
- **Time saved:** $O(2^n) \to O(n)$ due to linear loop.
- **Space effect:** $O(n) \to O(1)$ by keeping only two rolling variables.
- **Trade-off:** Loss of intermediate historical state if full path reconstruction is needed.

## 6. Core Idea
Because state $i$ only accesses states $i-1$ and $i-2$, earlier states ($i-3$ and below) become obsolete and can be discarded, collapsing space complexity to constant memory.

## 7. Pattern
- Pattern: 1D Linear Recurrence / Rolling Variables.
- Signals: "Rob houses without robbing adjacent", "climb stairs 1 or 2 steps", "decode ways", "min cost climbing stairs".

## 8. Data Structure Used
- Two or three primitive integer variables (`prev2`, `prev1`, `curr`). $O(1)$ space.

## 9. Invariant
At step $i$, `prev1` stores the globally optimal solution for prefix array `nums[0..i-1]`.

## 10. Dry Run
`nums = [2, 7, 9, 3, 1]`:
| $i$ | `nums[i]` | `prev2` | `prev1` | `current = max(nums[i] + prev2, prev1)` |
|---|---|---|---|---|
| 0 | 2 | 0 | 2 | - |
| 1 | 7 | 2 | 7 | $\max(7+0, 2) = 7$ |
| 2 | 9 | 2 | 7 | $\max(9+2, 7) = 11$ |
| 3 | 3 | 7 | 11 | $\max(3+7, 11) = 11$ |
| 4 | 1 | 11 | 11 | $\max(1+11, 11) = 12$ |

## 11. Edge Cases
- Array length 0 or 1: handle via early boundary checks.
- Circular houses (House Robber II): run 1D DP twice—once on `[0..n-2]` and once on `[1..n-1]`.
- Integer overflow on counting problems: use modulo arithmetic (`10^9 + 7`).

## 12. Correctness
By mathematical induction: At each step $i$, the decision to include `nums[i]` precludes `nums[i-1]`, meaning the best solution including `nums[i]` must build on optimal `dp[i-2]`. Taking the max over both possibilities guarantees global optimality.

## 13. Time Complexity
- Best / Average / Worst: $O(n)$ linear time single pass.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ using two rolling scalar variables.

## 15. Can It Be Optimized?
$O(n)$ time and $O(1)$ space is asymptotically optimal. For linear recurrences with constant coefficients, Matrix Exponentiation can solve the $n$-th state in $O(k^3 \log n)$ time.

## 16. When Should I Use This Algorithm?
- Problems where decision at index $i$ depends on decisions made at $i-1$ or $i-2$.
- Counting ways to reach a target step or position.
- Maximizing value along a linear sequence with non-adjacent constraints.
- Decoding encoded string sequences.

## 17. When Should I NOT Use It?
- Problem requires choices dependent on arbitrary previous indices $j < i$ (use Longest Increasing Subsequence with binary search or 2D DP).
- Multiple independent capacity constraints (use Knapsack).
- Decisions depend on two simultaneous strings (use 2D subsequences DP).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic 1D DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [DP Overview](../README.md) | [2D Grid DP](../2d-grid/README.md)
