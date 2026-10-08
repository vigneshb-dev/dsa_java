# Knapsack DP

> Capacity-constrained subset selection: 0/1 knapsack, unbounded knapsack, and bounded variants.

## 1. Overview
Knapsack DP solves constrained resource allocation problems where items with specific weights and values are chosen to maximize total value without exceeding capacity `W`. In 0/1 Knapsack, each item may be selected at most once (requiring right-to-left 1D array iteration). In Unbounded Knapsack, items may be chosen infinitely many times (allowing left-to-right 1D array iteration).

## 2. Time & Space Complexity
| Knapsack Variant | State Representation | Time Complexity | Space (2D / 1D) |
| :--- | :--- | :--- | :--- |
| 0/1 Knapsack | `dp[w] = max(dp[w], dp[w - wt] + val)` (reverse) | O(n * W) | O(n * W) / O(W) |
| Unbounded Knapsack | `dp[w] = max(dp[w], dp[w - wt] + val)` (forward) | O(n * W) | O(n * W) / O(W) |
| Subset Sum / Target Sum | `dp[w] = dp[w] || dp[w - num]` (reverse) | O(n * Target) | O(Target) |
| Coin Change 2 (Combinations) | `dp[w] += dp[w - coin]` (forward) | O(n * Amount) | O(Amount) |

Knapsack is pseudo-polynomial: runtime depends on the numeric magnitude of capacity W rather than the number of bits in input.

## 3. When to Use
- Selecting a subset of items to achieve an exact target sum or maximum value under capacity.
- Partitioning an array into two subsets with equal sums.
- Coin change problems (fewest coins or total combinations).
- Problems with 'take it or leave it' constraints.

## 4. When NOT to Use
- Capacity W is excessively large (e.g., W = 10^9, making array allocation impossible; use meet-in-the-middle or branch-and-bound).
- Items can be divided fractionally (use Greedy based on value-to-weight ratio).
- Item weights are continuous real numbers.

## 5. Why It Works
In 0/1 Knapsack, iterating capacity `w` in reverse (`W down to weight[i]`) ensures that `dp[w - weight[i]]` has not yet incorporated the current item `i`. In Unbounded Knapsack, iterating forwards allows an item to be selected repeatedly across increasing capacities.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (All Subsets) | Test all 2^n item inclusion subsets | O(2^n) | O(n) stack |
| Knapsack DP | Tabulate maximum value for each capacity up to W | O(n * W) | O(W) |

Knapsack DP trades memory proportional to capacity W to avoid exploring an exponential number of subsets.

## 7. Data Structures Used Here
- `int[] dp`: Array of size `W + 1` holding optimal values per capacity.
- `boolean[] dp`: Boolean array for subset sum reachability.

## 8. Core Template (Java)
```java
// 0/1 Knapsack standard 1D space-optimized pattern
int knapsack01(int[] weights, int[] values, int W) {
    int[] dp = new int[W + 1];
    for (int i = 0; i < weights.length; i++) {
        int wt = weights[i], val = values[i];
        for (int w = W; w >= wt; w--) { // Reverse iteration for 0/1
            dp[w] = Math.max(dp[w], dp[w - wt] + val);
        }
    }
    return dp[W];
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [2D Grid Dynamic Programming](../2d-grid/README.md) | [Subsequences DP](../subsequences/README.md)
