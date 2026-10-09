# Backtracking
> Exhaustive state-space tree search with incremental candidate construction and early constraint pruning.

## 1. Overview
Backtracking is a refined brute-force technique that incrementally builds candidates for solutions to a combinatorial problem and abandons ("backtracks" from) a candidate as soon as it determines the candidate cannot lead to a valid solution. It traverses an implicit state-space decision tree using depth-first search.

## 2. Input / Output
- Input: A candidate set of elements and problem constraints (e.g. `nums = [1, 2, 3]`).
- Output: A collection of all valid configurations (e.g. all 6 permutations: `[[1,2,3], [1,3,2], [2,1,3], ...]`).

## 3. Constraints
- Small input bounds due to combinatorial explosion:
  - Subsets ($O(2^n)$): $n \le 20$.
  - Permutations ($O(n!)$): $n \le 10$.
  - Grid puzzles (Sudoku, N-Queens): $N \le 12$.

## 4. Brute-Force Approach
- Idea: Generate all possible configurations in the universe first, then validate each one at the end.
- Pseudocode: Generate all $N^N$ board placements, then test if any queens attack each other.
- Time: $O(N^N)$ or $O(2^N)$; Space: $O(N)$.

## 5. Optimal Approach
- Idea: Choose $\to$ Explore $\to$ Un-choose (Backtrack), pruning invalid branches immediately before descending.
```java
// Reusable Combinatorial Backtracking Skeleton
import java.util.*;

public class BacktrackingTemplate {
    public static List<List<Integer>> solve(int[] nums) {
        List<List<Integer>> results = new ArrayList<>();
        List<Integer> currentPath = new ArrayList<>();
        boolean[] used = new boolean[nums.length];
        backtrack(nums, 0, currentPath, used, results);
        return results;
    }

    private static void backtrack(int[] nums, int start, List<Integer> path,
                                  boolean[] used, List<List<Integer>> results) {
        // 1. Goal / Base condition met
        if (path.size() == nums.length) { // Or target criteria
            results.add(new ArrayList<>(path)); // Deep copy path snapshot
            return;
        }

        // 2. Iterate through candidate choices
        for (int i = start; i < nums.length; i++) {
            // Pruning / constraint filter
            if (used[i]) continue;
            // Prune duplicate candidates if sorted
            if (i > start && nums[i] == nums[i - 1] && !used[i - 1]) continue;

            // Make Choice
            used[i] = true;
            path.add(nums[i]);

            // Explore Recursively
            backtrack(nums, i + 1, path, used, results);

            // Backtrack (Un-choose)
            path.remove(path.size() - 1);
            used[i] = false;
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Avoids generating and checking millions of dead-end configurations whose initial prefix already violates constraints.
- **Cases skipped:** If placing a Queen at `(row, col)` creates a conflict, the entire subtree of all $(N - row)$ subsequent Queen placements is skipped immediately.
- **Shortcuts / tricks used:** Constraint checking at each node; sorting candidates upfront to prune identical duplicate branches in constant time.
- **Time saved:** Exponential reduction in explored states (e.g. from $N^N$ down to valid board configurations).
- **Space effect:** Operates with $O(N)$ working space by reusing a single mutable `path` list and reverting modifications on backtrack.
- **Trade-off:** Incurs recursion function call overhead and snapshot copying (`new ArrayList<>(path)`).

## 6. Core Idea
Construct solutions one decision at a time. The moment a partial candidate violates constraints, prune that branch immediately and backtrack up the decision tree to test alternative choices.

## 7. Pattern
- Pattern: State-Space Decision Tree Search (Choose-Explore-Unchoose).
- Signals: "Find all combinations", "generate permutations", "solve Sudoku / N-Queens", "word search in grid", "partition string into palindromes".

## 8. Data Structure Used
- Dynamic List `List<T> path` or primitive array for current path state.
- Boolean array `boolean[] used` or bitmask for visited tracking.
- Output collection `List<List<T>>` for accumulating valid solution snapshots.

## 9. Invariant
Before and after the recursive call at depth $d$, the state of `path` and `used` is strictly identical (state mutation is completely undone upon return).

## 10. Dry Run
Subsets of `[1, 2]`:
| Step | Action | `path` | `results` |
|---|---|---|---|
| 1 | Add empty | `[]` | `[[]]` |
| 2 | Choose 1 | `[1]` | `[[], [1]]` |
| 3 | Choose 2 | `[1, 2]` | `[[], [1], [1, 2]]` |
| 4 | Unchoose 2 | `[1]` | - |
| 5 | Unchoose 1 | `[]` | - |
| 6 | Choose 2 | `[2]` | `[[], [1], [1, 2], [2]]` |
| 7 | Unchoose 2 | `[]` | Finished |

## 11. Edge Cases
- Forgetting to make a deep copy (`new ArrayList<>(path)`), saving empty paths.
- Forgetting to backtrack (state leaks across sibling branches).
- Duplicate elements in input creating duplicate answers (requires sorting and `nums[i] == nums[i-1]` pruning).

## 12. Correctness
By complete tree exploration with validity pruning: Every leaf reached satisfies all constraints because invalid branches were halted early, and every valid configuration is reachable by exhaustive branch enumeration.

## 13. Time Complexity
- Permutations: $O(n \cdot n!)$ where $n!$ leaves are reached and copying takes $O(n)$.
- Subsets / Combinations: $O(n \cdot 2^n)$.
- Sudoku / N-Queens: $O(k^n)$ worst case, bounded by pruning.

## 14. Space Complexity
- Auxiliary Space: $O(n)$ recursion stack and path list storage (excluding output space).

## 15. Can It Be Optimized?
Can be optimized using bitmasks for visited flags (`O(1)` operations), sorting inputs to fail fast, or Dancing Links (Algorithm X) for exact cover problems.

## 16. When Should I Use This Algorithm?
- Problems explicitly asking for "all valid solutions" or "all permutations / combinations".
- Constraint satisfaction puzzles (Sudoku, Crosswords, N-Queens).
- Finding whether ANY path exists in a maze or word search grid.
- Partitioning collections into subsets matching exact target sums.
- Generating all valid parenthesis strings.

## 17. When Should I NOT Use It?
- Only the COUNT of solutions or an OPTIMAL value (min/max) is requested without requiring path reconstruction (use Dynamic Programming).
- Input size $n > 30$ (backtracking will time out; look for Greedy or DP).
- Graphs with uniform edge weights seeking shortest path (use BFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic backtracking problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [04 - Recursion](../04-recursion/README.md) | [06 - Divide and Conquer](../06-divide-and-conquer/README.md)
