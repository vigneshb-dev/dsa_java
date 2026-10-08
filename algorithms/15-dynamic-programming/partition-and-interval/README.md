# Partition and Interval DP

> Optimal splitting or interval merging where state represents contiguous range `dp[i][j]`.

## 1. Overview
Partition and Interval Dynamic Programming operates on contiguous ranges `[i, j]` of an array or string. The state `dp[i][j]` represents the optimal cost or validity for the sub-range from index `i` to `j`, typically computed by iterating over all possible split points `k` between `i` and `j`. Classic applications include Matrix Chain Multiplication, Burst Balloons, and Palindrome Partitioning.

## 2. Time & Space Complexity
| Problem / Variant | State Transition | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Matrix Chain Multiplication | `dp[i][j] = min(dp[i][k] + dp[k+1][j] + cost)` | O(n^3) | O(n^2) |
| Burst Balloons | `dp[i][j] = max(dp[i][k] + dp[k][j] + coins)` | O(n^3) | O(n^2) |
| Palindrome Partitioning II | Find min cuts to make substrings palindromic | O(n^2) | O(n^2) |
| Minimum Cost Tree From Leaf | Range split optimization | O(n^3) | O(n^2) |

The evaluation order must iterate by increasing interval length `len = 1 to n` so that smaller sub-intervals are computed before longer enclosing intervals.

## 3. When to Use
- Problem involves merging adjacent elements or splitting ranges at an optimal midpoint.
- The final step can be viewed as picking the last element/operation to occur.
- Subproblems are naturally bounded by contiguous subsegments `[i, j]`.
- Array size is moderate (n <= 500, suitable for O(n^3) runtime).

## 4. When NOT to Use
- Input size is large (n >= 2,000, where O(n^3) exceeds time limits).
- Intervals can be reordered arbitrarily (non-contiguous intervals require set/bitmask states).
- Optimal split point has a greedy monotonicity that allows O(n log n) divide-and-conquer optimization without full DP.

## 5. Why It Works
By choosing the last operation in range `[i, j]` to occur at split index `k`, the subproblems `[i, k]` and `[k + 1, j]` become completely independent. Evaluating shorter ranges first ensures that all dependent sub-ranges are fully resolved before combining.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Recursive Exhaustive Partition | Try all split points recursively at every level | Exponential O(2^n or Catalan) | O(n) stack |
| Interval DP Tabulation | Solve subproblems by range length `len` from 2 to n | O(n^3) | O(n^2) |

Interval DP computes each sub-interval `[i, j]` exactly once, dropping Catalan factorial search trees to polynomial O(n^3).

## 7. Data Structures Used Here
- `int[][] dp`: 2D table storing optimal values for range `[i, j]`.

## 8. Core Template (Java)
```java
// Standard Interval DP Loop Skeleton (increasing length)
int intervalDP(int n) {
    int[][] dp = new int[n][n];
    for (int len = 2; len <= n; len++) { // length of interval
        for (int i = 0; i <= n - len; i++) {
            int j = i + len - 1;
            dp[i][j] = Integer.MAX_VALUE;
            for (int k = i; k < j; k++) { // split point
                int cost = dp[i][k] + dp[k + 1][j] /* + transition cost */;
                dp[i][j] = Math.min(dp[i][j], cost);
            }
        }
    }
    return dp[0][n - 1];
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [Subsequences DP](../subsequences/README.md) | [Bitmask Dynamic Programming](../bitmask/README.md)
