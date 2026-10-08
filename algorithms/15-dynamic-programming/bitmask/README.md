# Bitmask Dynamic Programming

> State compression using binary bit vectors as table indices for small-universe subsets.

## 1. Overview
Bitmask Dynamic Programming uses integer binary representations as compact state descriptions representing subsets of visited items. For a set of size `n` (typically `n <= 20`), an integer mask from `0` to `2^n - 1` encodes whether the k-th element is present via its k-th bit. This enables polynomial-exponential algorithms for NP-hard challenges like the Traveling Salesperson Problem (TSP) and Assignment Problems.

## 2. Time & Space Complexity
| Problem / Variant | State Representation | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Traveling Salesperson (TSP) | `dp[mask][u]` | O(n^2 * 2^n) | O(n * 2^n) |
| Matchsticks to Square | `dp[mask]` | O(n * 2^n) | O(2^n) |
| Partition to K Equal Sum Subsets | `dp[mask]` | O(n * 2^n) | O(2^n) |
| Number of Ways to Wear Hats | `dp[mask][hat]` | O(hats * 2^people) | O(2^people) |

A set size of n = 20 yields 2^20 ~= 1.05 * 10^6 states, fitting comfortably within memory and typical 1-second time limits.

## 3. When to Use
- Input constraint n is small (typically n <= 20).
- Every state requires tracking a subset of visited or assigned items.
- Hamiltonian path or tour optimization problems (TSP).
- Exact partition of an array into equal-sum groups.

## 4. When NOT to Use
- Input size n > 25 (2^26 exceeds memory limits and causes OutOfMemoryError).
- Items can be duplicated without bound (mask representation assumes boolean presence).
- Problem can be solved with greedy or polynomial matching algorithms (e.g., Hungarian algorithm for bipartite matching).

## 5. Why It Works
Bitwise operations (`mask | (1 << i)`, `mask & (1 << i)`) execute in single CPU instruction cycles. Representing subsets as primitive integers provides immediate O(1) table indexing into flat arrays.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Permutations) | Check all n! orderings of vertices | O(n!) | O(n) |
| Bitmask DP (Held-Karp) | Cache optimal cost for `(visited_mask, last_vertex)` | O(n^2 * 2^n) | O(n * 2^n) |

Held-Karp bitmask DP replaces factorial n! permutations with exponential O(n^2 * 2^n) subproblem caching.

## 7. Data Structures Used Here
- `int[][] dp`: 2D table where first index is integer `mask` in `[0, (1 << n) - 1]`.
- Bitwise operators (`&`, `|`, `^`, `<<`): State transitions.

## 8. Core Template (Java)
```java
// TSP Held-Karp Bitmask DP Skeleton
int tsp(int[][] dist, int n) {
    int[][] dp = new int[1 << n][n];
    for (int[] row : dp) Arrays.fill(row, 1_000_000_000);
    dp[1][0] = 0; // start at city 0 with mask 00...01

    for (int mask = 1; mask < (1 << n); mask++) {
        for (int u = 0; u < n; u++) {
            if ((mask & (1 << u)) == 0) continue;
            for (int v = 0; v < n; v++) {
                if ((mask & (1 << v)) != 0) continue;
                int nextMask = mask | (1 << v);
                dp[nextMask][v] = Math.min(dp[nextMask][v], dp[mask][u] + dist[u][v]);
            }
        }
    }
    return dp[(1 << n) - 1][0];
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [Partition and Interval DP](../partition-and-interval/README.md) | [Tree Dynamic Programming](../tree-dp/README.md)
