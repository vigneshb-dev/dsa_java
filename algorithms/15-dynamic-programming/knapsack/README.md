# Knapsack Dynamic Programming
> Constrained subset optimization under capacity limits, covering 0/1, Unbounded, and Bounded Knapsack variants.

## 1. Overview
The Knapsack problem family selects a subset of items, each with a given weight and value, to maximize total value without exceeding a specified capacity $W$. Depending on whether items can be chosen once (0/1 Knapsack), infinitely (Unbounded Knapsack), or a bounded count (Bounded Knapsack), recurrence loop directions determine state transitions.

## 2. Input / Output
- Input: Arrays `weights`, `values`, and integer `capacity` (e.g. `wt = [1, 3, 4]`, `val = [15, 50, 60]`, `W = 4`).
- Output: Maximum value attainable (e.g. `65` by taking items 0 and 1).

## 3. Constraints
- Number of items $N \le 1,000$; capacity $W \le 10^5$.
- Pseudo-polynomial complexity $O(N \cdot W)$ is required; if $W > 10^9$, standard knapsack DP times out (requires meet-in-the-middle or branch-and-bound).

## 4. Brute-Force Approach
- Idea: Generate all $2^N$ subsets of items and check each for maximum value within capacity.
- Pseudocode: Recursive include/exclude tree.
- Time: $O(2^N)$; Space: $O(N)$ recursion depth.

## 5. Optimal Approach
- Idea: Define `dp[w]` as maximum value with capacity $w$. In 0/1 Knapsack, iterate $w$ in **reverse** from $W$ down to $wt[i]$ to prevent using the same item multiple times. In Unbounded Knapsack, iterate $w$ **forward**.
```java
// Reusable Knapsack DP Template (0/1 vs Unbounded)
public class KnapsackTemplate {
    // 0/1 Knapsack: each item used at most once (REVERSE loop)
    public static int zeroOneKnapsack(int[] wt, int[] val, int W) {
        int[] dp = new int[W + 1];
        for (int i = 0; i < wt.length; i++) {
            for (int w = W; w >= wt[i]; w--) { // Reverse order prevents multi-use
                dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }
        return dp[W];
    }

    // Unbounded Knapsack: items can be used infinitely (FORWARD loop)
    public static int unboundedKnapsack(int[] wt, int[] val, int W) {
        int[] dp = new int[W + 1];
        for (int i = 0; i < wt.length; i++) {
            for (int w = wt[i]; w <= W; w++) { // Forward order allows multi-use
                dp[w] = Math.max(dp[w], val[i] + dp[w - wt[i]]);
            }
        }
        return dp[W];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Subsets with identical total weight and subset prefixes are collapsed into a single scalar maximum.
- **Cases skipped:** Branches where item weight exceeds current remaining capacity are never explored.
- **Shortcuts / tricks used:** Reverse inner loop allows 1D array space optimization without allocating an $N \times W$ matrix.
- **Time saved:** $O(2^N) \to O(N \cdot W)$ pseudo-polynomial time.
- **Space effect:** $O(N \cdot W) \to O(W)$ 1D array space.
- **Trade-off:** Capacity $W$ must be reasonably small integer.

## 6. Core Idea
Every item presents a choice: exclude it (keep current best for capacity $w$) or include it (gain its value plus the best solution for remaining capacity $w - wt[i]$). Comparing both gives the optimal choice.

## 7. Pattern
- Pattern: Subset Selection under Capacity Budget.
- Signals: "Partition equal subset sum", "target sum with +/- signs", "coin change (fewest coins / total ways)", "ones and zeroes (2D capacity knapsack)".

## 8. Data Structure Used
- 1D primitive array `int[W + 1]` (or `long[]` for large totals).

## 9. Invariant
In 0/1 Knapsack, after processing item $i$, `dp[w]` contains the optimal value using a subset of items from index $0$ to $i$ with capacity $\le w$.

## 10. Dry Run
0/1 Knapsack with items `(wt: 1, val: 15)` and `(wt: 3, val: 50)`, $W = 4$:
| Item Processed | $w = 4$ | $w = 3$ | $w = 2$ | $w = 1$ | $w = 0$ |
|---|---|---|---|---|---|
| Init | 0 | 0 | 0 | 0 | 0 |
| Item 1 (1, 15) | 15 | 15 | 15 | 15 | 0 |
| Item 2 (3, 50) | $\max(15, 50+15)=65$ | $\max(15, 50+0)=50$ | 15 | 15 | 0 |

Result: `dp[4] = 65`.

## 11. Edge Cases
- Item weight $> W$: loop guard `w >= wt[i]` automatically bypasses oversized items.
- Capacity $W = 0$: returns 0.
- Negative weights: standard knapsack DP breaks down (requires shifting or graph shortest paths).

## 12. Correctness
By induction on item prefix $i$: If `dp` correctly holds the optimal values for items $0 \dots i-1$, then for item $i$, reverse iteration guarantees `dp[w - wt[i]]` has not yet incorporated item $i$, precisely modeling the choice of adding item $i$ at most once.

## 13. Time Complexity
- Best / Average / Worst: $O(N \cdot W)$ pseudo-polynomial time.

## 14. Space Complexity
- Auxiliary Space: $O(W)$ using 1D space optimization.

## 15. Can It Be Optimized?
If item values are small ($V_{total} \ll W$), redefine state as `dp[v] = min weight to achieve value v` in $O(N \cdot V)$ time. Bounded Knapsack can be optimized to $O(N \cdot W \log K)$ via binary splitting.

## 16. When Should I Use This Algorithm?
- 0/1 Knapsack (items used at most once).
- Partitioning arrays into two subsets with equal sum.
- Target Sum problems (reducing to subset sum).
- Unbounded Knapsack (Coin Change: ways or min coins).
- Bounded Knapsack with item counts.

## 17. When Should I NOT Use It?
- Fractional Knapsack (items can be split; use Greedy in $O(N \log N)$).
- Capacity $W$ is massive ($W > 10^9$) and $N \le 40$ (use Meet-in-the-Middle in $O(2^{N/2})$).
- Non-integer continuous weights.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic knapsack DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [2D Grid DP](../2d-grid/README.md) | [Subsequences DP](../subsequences/README.md)
