# Backtracking

> Systematic state-space tree exploration with pruning and decision restoration.

## 1. Overview
Backtracking is an algorithmic paradigm that systematically searches for solutions by exploring candidate paths in a decision tree. If a candidate path violates problem constraints or fails to produce a valid solution, the algorithm 'backtracks' by undoing the most recent choice and trying alternative branches. Pruning invalid branches early avoids exhaustive enumeration of unproductive search spaces.

## 2. Time & Space Complexity
| Problem Pattern | State Space Tree Size | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Subsets (Power Set) | 2^n states | O(n * 2^n) | O(n) call stack |
| Permutations | n! states | O(n * n!) | O(n) call stack |
| Combinations (n choose k) | C(n, k) states | O(k * C(n, k)) | O(k) call stack |
| N-Queens / Sudoku | Heavily pruned factorial space | O(n!) worst, much faster avg | O(n) |

The extra factor of `n` or `k` in complexity stems from copying candidate lists into final output answer structures.

## 3. When to Use
- Find all valid combinations, permutations, subsets, or partitions.
- Constraint satisfaction problems (N-Queens, Sudoku solver, Word Search).
- Partitioning strings into valid palindromes or IP addresses.
- Input constraints are small (typically n <= 20 for subsets, n <= 12 for permutations).

## 4. When NOT to Use
- Only the optimal numerical value (min/max count) is needed without generating configurations (use DP or Greedy).
- Input size is large (n >= 10^3), where factorial or exponential search causes time-limit exceeded.
- Subproblems are independent and have greedy choice properties.

## 5. Why It Works
Backtracking formalizes Depth-First Search over an implicit decision tree. By ensuring the state after returning from a recursive call is identical to the state before the call ('Choose -> Explore -> Unchoose'), a single mutable data structure can be reused across all paths.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Generate All & Validate | Generate all n^n configurations, then check validity | O(n^n) | O(n) |
| Backtracking with Pruning | Check constraints at each step; prune invalid branches immediately | O(n!) or less | O(n) |

Pruning eliminates entire subtrees of invalid candidates as early as possible without exploring their descendants.

## 7. Data Structures Used Here
- `List<Integer>` / `ArrayList<Integer>`: Mutable path buffer holding current decisions.
- `boolean[] visited`: Lookup array for tracking used elements in permutations.

## 8. Core Template (Java)
```java
// Standard Backtracking Skeleton: Choose -> Explore -> Unchoose
void backtrack(int start, int[] nums, List<Integer> current, List<List<Integer>> result) {
    // 1. Goal state check
    result.add(new ArrayList<>(current));

    // 2. Iterate candidates
    for (int i = start; i < nums.length; i++) {
        // Pruning condition (if any)
        current.add(nums[i]);            // Choose
        backtrack(i + 1, nums, current, result); // Explore
        current.remove(current.size() - 1); // Unchoose
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Recursion](../04-recursion/README.md) | [Divide and Conquer](../06-divide-and-conquer/README.md)
