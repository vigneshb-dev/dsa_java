# Partition & Interval Dynamic Programming
> Solving optimal subsegment boundaries and merge orders over ranges [i, j] via split-point minimization.

## 1. Overview
Interval Dynamic Programming solves optimization problems over continuous subsegments $[i, j]$ by partitioning the range at an optimal internal split point $k \in [i, j-1]$. Classic problems include Matrix Chain Multiplication, Burst Balloons, Minimum Cost Tree From Leaf Values, and Palindrome Partitioning.

## 2. Input / Output
- Input: An array of values, dimensions, or string (e.g. balloon values `[3, 1, 5, 8]`).
- Output: Optimal scalar metric (e.g. maximum coins collected `167`).

## 3. Constraints
- Sequence length $N \le 500 \implies O(N^3)$ cubic operations $\approx 1.25 \times 10^7$.
- Subproblems naturally depend on smaller length intervals ($j - i < len$).

## 4. Brute-Force Approach
- Idea: Test all possible parenthesizations or split orders recursively.
- Pseudocode: Catalan number tree search $C_N = \frac{1}{N+1}\binom{2N}{N}$.
- Time: $O(4^N / N^{1.5})$; Space: $O(N)$ call stack.

## 5. Optimal Approach
- Idea: Solve intervals in order of increasing length $len$ from 1 to $N$. For interval $[i, j]$, iterate all possible split points $k$ between $i$ and $j-1$, combining solutions of $[i, k]$ and $[k+1, j]$.
```java
// Reusable Interval DP Skeleton (Matrix Chain Multiplication / Range DP)
public class IntervalDPTemplate {
    public static int solve(int[] arr) {
        int n = arr.length;
        // dp[i][j] stores optimal answer for range [i, j]
        int[][] dp = new int[n][n];

        // 1. Iterate over interval length: 2 to n
        for (int len = 2; len <= n; len++) {
            for (int i = 0; i <= n - len; i++) {
                int j = i + len - 1;
                dp[i][j] = Integer.MAX_VALUE;

                // 2. Iterate over partition split point k
                for (int k = i; k < j; k++) {
                    int cost = dp[i][k] + dp[k + 1][j] + computeCost(arr, i, k, j);
                    dp[i][j] = Math.min(dp[i][j], cost);
                }
            }
        }

        return dp[0][n - 1];
    }

    private static int computeCost(int[] arr, int i, int k, int j) {
        return 0; // Problem-specific combine cost
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Prevents recomputing the optimal cost of smaller sub-ranges $[i, k]$ across different parenthesization trees.
- **Cases skipped:** Suboptimal split points for a range $[i, j]$ are discarded by the minimization operator.
- **Shortcuts / tricks used:** Solving by increasing interval length guarantees that both child sub-intervals $[i, k]$ and $[k+1, j]$ are strictly smaller than $len$ and thus already computed.
- **Time saved:** $O(4^N) \to O(N^3)$.
- **Space effect:** Allocates $O(N^2)$ memory for the interval table.
- **Trade-off:** Incurs cubic time complexity, limiting feasibility to $N \le 500$.

## 6. Core Idea
Every interval $[i, j]$ can be decomposed into two contiguous sub-intervals $[i, k]$ and $[k+1, j]$ by selecting an optimal split point $k$. Ordering the loops by interval length guarantees smaller intervals are always resolved first.

## 7. Pattern
- Pattern: Interval / Range Split DP.
- Signals: "Matrix chain multiplication", "burst balloons", "minimum cost to merge stones", "remove boxes", "strange printer", "triangulation of convex polygon".

## 8. Data Structure Used
- 2D primitive array `int[N][N]`.

## 9. Invariant
When calculating range $[i, j]$ of length $L$, all sub-intervals of length strictly less than $L$ already store their globally optimal values.

## 10. Dry Run
Interval lengths on 3 elements ($len = 1$ base cases = 0):
| $len$ | Interval $[i, j]$ | Possible Splits $k$ | Sub-Intervals Evaluated | Computed `dp[i][j]` |
|---|---|---|---|---|
| 2 | `[0, 1]` | $k = 0$ | `[0, 0]` and `[1, 1]` | $cost(0, 0, 1)$ |
| 2 | `[1, 2]` | $k = 1$ | `[1, 1]` and `[2, 2]` | $cost(1, 1, 2)$ |
| 3 | `[0, 2]` | $k = 0, 1$ | `[0, 0]+[1, 2]` vs `[0, 1]+[2, 2]` | $\min(\text{split } 0, \text{split } 1)$ |

## 11. Edge Cases
- Intervals of length 1: base case initialized to 0 or point value.
- Invalid split points: loop bounds must ensure $i \le k < j$.
- Burst Balloons inverse trick: define $k$ as the **last** balloon to burst in range $[i, j]$ to make subproblems independent.

## 12. Correctness
By induction on interval length: Base intervals of length 1 have known costs. If all intervals of length $< L$ are optimal, the best configuration for interval of length $L$ must have some last boundary cut at some $k$. Testing all valid $k \in [i, j-1]$ guarantees finding this optimum.

## 13. Time Complexity
- Best / Average / Worst: $O(N^3)$ due to three nested loops: length ($N$), start index ($N$), and split point ($N$).

## 14. Space Complexity
- Auxiliary Space: $O(N^2)$ for 2D DP table.

## 15. Can It Be Optimized?
Knuth's Optimization reduces time from $O(N^3)$ to $O(N^2)$ if the cost function satisfies the quadrangle inequality ($opt[i][j-1] \le opt[i][j] \le opt[i+1][j]$).

## 16. When Should I Use This Algorithm?
- Optimal parenthesization or merging of adjacent items.
- Range segmentation where subproblems interact across an internal boundary.
- Bursting balloons or cutting sticks with minimum cost.
- Parsing context-free grammars (CYK algorithm).

## 17. When Should I NOT Use It?
- $N > 500$ (cubic time will exceed typical 1-second execution limits).
- Subproblems do not partition into contiguous ranges (use Bitmask DP).
- Greedy interval scheduling applies without split costs.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic interval DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Subsequences DP](../subsequences/README.md) | [Bitmask DP](../bitmask/README.md)
