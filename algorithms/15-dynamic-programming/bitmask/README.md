# Bitmask Dynamic Programming
> Exponential state compression using binary integer bitmasks to represent subset selections and permutations.

## 1. Overview
Bitmask Dynamic Programming represents subsets of a universe of elements using the individual binary bits of an integer (where bit $i$ is 1 if element $i$ is included, and 0 otherwise). It solves NP-hard problems over small sets like the Traveling Salesperson Problem (TSP), optimal job assignment, and Hamiltonian paths in $O(N^2 2^N)$ rather than $O(N!)$.

## 2. Input / Output
- Input: An $N \times N$ cost or distance matrix (e.g. TSP graph of distances between $N$ cities).
- Output: Minimum total travel cost or matching score (e.g. minimum tour distance `35`).

## 3. Constraints
- Set size $N \le 20$. For $N = 20$, $2^{20} \approx 10^6$ states, total operations $N^2 2^N \approx 8 \times 10^7$.
- If $N > 22$, memory and time exceed standard competition and JVM limits.

## 4. Brute-Force Approach
- Idea: Try all possible permutations of visiting $N$ cities.
- Pseudocode: Recursive permutation generation.
- Time: $O(N!)$; Space: $O(N)$ recursion depth. (For $N = 20$, $20! \approx 2.4 \times 10^{18}$, hopelessly impossible).

## 5. Optimal Approach
- Idea: Define `dp[mask][u]` as the minimum cost to visit the subset of cities encoded by `mask`, ending at city `u`. Transition to next unvisited city $v$ by setting bit $v$: `mask | (1 << v)`.
```java
// Reusable Bitmask DP Template (Held-Karp TSP Skeleton)
import java.util.Arrays;

public class BitmaskDPTemplate {
    public static int tsp(int[][] dist) {
        int n = dist.length;
        int totalStates = 1 << n;
        int[][] dp = new int[totalStates][n];

        for (int[] row : dp) Arrays.fill(row, 1_000_000_000);
        dp[1][0] = 0; // Base case: mask containing only starting city 0 (bit 0 = 1)

        // Iterate through all bitmasks in numerical order (topological order)
        for (int mask = 1; mask < totalStates; mask++) {
            for (int u = 0; u < n; u++) {
                // If city u is not present in the current mask, skip
                if ((mask & (1 << u)) == 0 || dp[mask][u] >= 1_000_000_000) continue;

                // Try transitioning to an unvisited city v
                for (int v = 0; v < n; v++) {
                    if ((mask & (1 << v)) == 0) { // City v not yet in mask
                        int nextMask = mask | (1 << v);
                        dp[nextMask][v] = Math.min(dp[nextMask][v], dp[mask][u] + dist[u][v]);
                    }
                }
            }
        }

        // Close the tour by returning to origin city 0
        int finalMask = totalStates - 1;
        int minTour = Integer.MAX_VALUE;
        for (int u = 1; u < n; u++) {
            minTour = Math.min(minTour, dp[finalMask][u] + dist[u][0]);
        }
        return minTour;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Subproblems with the same subset of visited cities and the same current endpoint are merged into a single minimum cost, eliminating factorial redundancy.
- **Cases skipped:** If two paths visit the same subset of cities and both end at city $u$, only the cheaper path is retained; the more expensive path is discarded permanently.
- **Shortcuts / tricks used:** Bitwise operations (`mask & (1 << v)` to test membership, `mask | (1 << v)` to insert) execute in a single processor instruction cycle.
- **Time saved:** $O(N!) \to O(N^2 \cdot 2^N)$; for $N = 20$, reduces operations from $10^{18}$ down to $10^7$.
- **Space effect:** Allocates $O(N \cdot 2^N)$ integers.
- **Trade-off:** Strictly bounded to small inputs $N \le 20$ due to exponential memory scaling.

## 6. Core Idea
A subset of size $N$ can be uniquely represented by an integer in $[0, 2^N - 1]$. Since every integer strictly increases when adding a new element (setting a 0 bit to 1), looping numerically through integers $1 \dots 2^N - 1$ guarantees a perfect topological order.

## 7. Pattern
- Pattern: Subset State Compression / Held-Karp Algorithm.
- Signals: "Small N (N <= 20)", "visit all nodes / cities", "optimal matching of N tasks to N workers", "partition array into k subsets with equal sum", "find Hamiltonian cycle".

## 8. Data Structure Used
- 2D primitive integer array `int[1 << N][N]`.
- Bitwise operators: `&`, `|`, `^`, `<<`, `>>`.

## 9. Invariant
When processing `mask`, all sub-masks (strictly smaller subsets) have already been fully computed and contain their optimal values.

## 10. Dry Run
Bitmask states for $N = 3$:
| Integer `mask` | Binary Representation | Cities Included | Allowed Next Cities ($v$) |
|---|---|---|---|
| 1 | `001` | `{0}` | 1, 2 |
| 3 | `011` | `{0, 1}` | 2 |
| 5 | `101` | `{0, 2}` | 1 |
| 7 | `111` | `{0, 1, 2}` | None (Tour complete) |

## 11. Edge Cases
- 32-bit shift overflow: `1 << 31` causes integer overflow; for $N > 30$, use `1L << n` (though $N > 25$ is already infeasible for DP memory).
- Unreachable states: check against infinity sentinel before transitioning.
- Operator precedence: bitwise operators have lower precedence than comparison operators; always wrap in parentheses: `(mask & (1 << i)) == 0`.

## 12. Correctness
By induction on the number of set bits (Hamming weight): Transitions only go from masks with $k$ set bits to masks with $k + 1$ set bits. Since cost is non-negative and subproblem states encapsulate the exact set of visited nodes, optimal substructure holds.

## 13. Time Complexity
- Best / Average / Worst: $O(N^2 \cdot 2^N)$ total operations. There are $2^N$ masks, $N$ ending nodes, and $N$ transition choices.

## 14. Space Complexity
- Auxiliary Space: $O(N \cdot 2^N)$ for the DP table (approx. 80 MB for $N = 20$).

## 15. Can It Be Optimized?
SOS DP (Sum Over Subsets) optimizes submask iteration from $O(3^N)$ to $O(N \cdot 2^N)$ using multidimensional prefix sums over bits.

## 16. When Should I Use This Algorithm?
- Traveling Salesperson Problem on small graphs ($N \le 20$).
- Assignment problem (matching $N$ workers to $N$ tasks).
- Finding shortest Hamiltonian path or cycle.
- Partitioning $N$ elements into $K$ subsets.
- Maximum compatibility score matching.

## 17. When Should I NOT Use It?
- $N > 22$ (memory and time explode exponentially; look for approximation, heuristics, or branch-and-bound).
- Bipartite matching without non-linear costs (use Hopcroft-Karp in polynomial time).
- Problems with linear or tree structures (use 1D or Tree DP).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic bitmask DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Partition & Interval DP](../partition-and-interval/README.md) | [Tree DP](../tree-dp/README.md)
