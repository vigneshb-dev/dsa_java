# Greedy
> Making locally optimal choices at each decision stage to construct a mathematically provable global optimum.

## 1. Overview
The Greedy algorithm paradigm constructs an optimal solution piece by piece, always choosing the immediate next choice that offers the most obvious and immediate benefit (greedy choice property). Because it never reconsiders or backtracks on previous decisions, it operates with exceptional speed when the problem exhibits both the greedy choice property and optimal substructure.

## 2. Input / Output
- Input: A collection of candidate items with associated weights, values, intervals, or costs (e.g. intervals `[[1, 2], [2, 3], [3, 4]]`).
- Output: An optimal aggregate value or selected subset (e.g. maximum count of non-overlapping activities `3`).

## 3. Constraints
- Scales up to $n \le 10^6$, typically bounded by an initial $O(n \log n)$ sort or priority queue heap operations.
- Problem must satisfy the Greedy Choice Property (local choice is part of some global optimum) and Optimal Substructure.

## 4. Brute-Force Approach
- Idea: Test all possible combinations or subsets to find the combination yielding the maximum or minimum objective.
- Pseudocode: Generate all $2^n$ subsets and evaluate feasibility for each.
- Time: $O(2^n)$; Space: $O(n)$.

## 5. Optimal Approach
- Idea: Sort candidates according to a rigorous greedy criterion (e.g. earliest finishing time, highest unit value, lowest cost) and commit to choices in a single forward pass without looking back.
```java
// Reusable Greedy Activity Selection / Scheduling Skeleton
import java.util.*;

public class GreedyTemplate {
    static class Item {
        int weight, value;
        double ratio;
        Item(int w, int v) {
            this.weight = w; this.value = v;
            this.ratio = (double) v / w;
        }
    }

    // Fractional Knapsack (Standard Greedy Paradigm)
    public static double getMaxValue(Item[] items, int capacity) {
        // 1. Sort by greedy metric descending (highest value-to-weight ratio)
        Arrays.sort(items, (a, b) -> Double.compare(b.ratio, a.ratio));

        double totalValue = 0.0;
        int currentCapacity = capacity;

        // 2. Iterate and commit greedily
        for (Item item : items) {
            if (currentCapacity >= item.weight) {
                // Take whole item
                currentCapacity -= item.weight;
                totalValue += item.value;
            } else {
                // Take fraction and finish
                totalValue += item.ratio * currentCapacity;
                break;
            }
        }

        return totalValue;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Completely eliminates exploring exponential alternative choices, branches, and combinations.
- **Cases skipped:** Rejects non-optimal candidate paths permanently without exploring them; once a local greedy decision is made, alternative candidates for that slot are never considered.
- **Shortcuts / tricks used:** Sorting by the correct priority metric ensures the current best choice is always at the head of the list or top of the heap.
- **Time saved:** $O(2^n) \to O(n \log n)$; replacing exhaustive combinatorial search with a sort and single linear sweep.
- **Space effect:** Operates with $O(1)$ to $O(n)$ working space without requiring large memoization tables.
- **Trade-off:** Greedy strategies only work for specific problem structures; if greedy choice property fails, the algorithm yields suboptimal results.

## 6. Core Idea
Make the locally optimal decision at every step. If the problem structure guarantees that local optimality cannot preclude global optimality (provable via exchange arguments), the greedy path leads directly to the true global optimum.

## 7. Pattern
- Pattern: Greedy Choice / Exchange Argument Selection.
- Signals: "Activity selection", "minimum spanning tree (Prim/Kruskal)", "Dijkstra's shortest path", "fractional knapsack", "Huffman coding", "gas station tour", "jump game".

## 8. Data Structure Used
- Sorting comparator or `PriorityQueue` (Min/Max Heap) to repeatedly extract the next best candidate in $O(\log n)$.

## 9. Invariant
At step $k$, the partial solution constructed so far can be extended to an optimal global solution.

## 10. Dry Run
Coin Change with canonical currency `[25, 10, 5, 1]` for amount `36`:
| Step | Remaining Amount | Available Coins | Greedy Choice | Coins Taken | Remaining Amount |
|---|---|---|---|---|---|
| 1 | 36 | 25, 10, 5, 1 | Take 25 | 1 x 25 | 11 |
| 2 | 11 | 10, 5, 1 | Take 10 | 1 x 10 | 1 |
| 3 | 1 | 5, 1 | Take 1 | 1 x 1 | 0 (Done: 3 coins) |

## 11. Edge Cases
- Currency systems that are non-canonical (e.g. coins `[1, 3, 4]` for amount `6`: greedy chooses `4 + 1 + 1` = 3 coins, whereas optimal is `3 + 3` = 2 coins; greedy fails, requires DP!).
- Ties in sorting metrics: verify whether secondary tie-breaking order affects correctness.
- Resource capacity of 0 or empty input array.

## 12. Correctness
Proved via **Exchange Argument**: Assume an optimal solution $OPT$ differs from greedy solution $G$ at the first choice. Show that replacing $OPT$'s choice with $G$'s choice yields a valid solution whose total value is at least as good as $OPT$. By induction, $G$ is optimal.

## 13. Time Complexity
- Best / Average / Worst: $O(n \log n)$ when sorting is required; $O(n)$ if input is pre-sorted or uses a single pass.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ to $O(n)$ (depending on sorting implementation and comparator).

## 15. Can It Be Optimized?
If the greedy choice can be extracted without sorting (e.g. running linear extremum tracking or counting sort for bounded keys), time reduces to $O(n)$.

## 16. When Should I Use This Algorithm?
- Interval scheduling / Activity selection (sort by end time).
- Minimum spanning tree (Kruskal's, Prim's).
- Single-source shortest paths with non-negative weights (Dijkstra's).
- Huffman data compression encoding.
- Fractional Knapsack (divisible items).
- Jump Game (track maximum reachable index).

## 17. When Should I NOT Use It?
- 0/1 Knapsack (items cannot be divided; use Dynamic Programming).
- General Coin Change with arbitrary denominations (use Dynamic Programming).
- Longest path in DAG or graph with negative weights (use DP or Bellman-Ford).
- Traveling Salesperson Problem (greedy nearest-neighbor yields poor approximations).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic greedy problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [13 - Cyclic Sort](../13-cyclic-sort/README.md) | [15 - Dynamic Programming](../15-dynamic-programming/README.md)
