# Dynamic Programming

> Optimizing recursive problems by breaking them into overlapping subproblems and caching intermediate states.

## 1. Overview
Dynamic Programming (DP) is an optimization technique that solves complex problems by breaking them into overlapping subproblems exhibiting optimal substructure. Rather than repeatedly recomputing solutions to identical subproblems, DP computes each subproblem solution once and stores it in a table (memoization for top-down, tabulation for bottom-up). It turns exponential brute-force recursions into polynomial-time solutions.

### DP Sub-Topic Folders

| Sub-folder | Description |
| :--- | :--- |
| [1d](1d/README.md) | Linear state transitions (Fibonacci, Climbing Stairs, House Robber) |
| [2d-grid](2d-grid/README.md) | Coordinate grid paths, minimum path sums, and matrix cost minimization |
| [knapsack](knapsack/README.md) | 0/1 Knapsack, Unbounded Knapsack, and Target Sum subsets |
| [subsequences](subsequences/README.md) | Longest Common Subsequence (LCS) and Longest Increasing Subsequence (LIS) |
| [partition-and-interval](partition-and-interval/README.md) | Interval DP, Matrix Chain Multiplication, and Palindrome Partitioning |
| [bitmask](bitmask/README.md) | State compression representing visited sets as integers (TSP, Assignment) |
| [tree-dp](tree-dp/README.md) | Subtree state propagation and re-rooting techniques on trees |

## 2. Time & Space Complexity
| Approach / Variant | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| Top-Down (Memoization) | O(Total Unique States * Transition Cost) | O(States) table + O(depth) stack |
| Bottom-Up (Tabulation) | O(Total Unique States * Transition Cost) | O(States) table |
| Space-Optimized Bottom-Up | O(Total Unique States * Transition Cost) | O(Previous Row / State) |

Space optimization reduces table dimensions whenever state `dp[i]` depends only on values from the immediately preceding step `dp[i-1]`.

## 3. When to Use
- Problem asks for optimal value (maximum profit, minimum cost, shortest steps, number of distinct ways).
- Problem exhibits overlapping subproblems (identical subproblems evaluated multiple times in recursion tree).
- Problem exhibits optimal substructure (optimal solution to the problem contains optimal solutions to subproblems).
- Greedy approaches fail because early local choices compromise future global payoffs.

## 4. When NOT to Use
- Subproblems are strictly independent without overlap (use simple Divide and Conquer like Merge Sort).
- Problem has the greedy choice property where locally optimal choices provably yield the global optimum (use Greedy for lower complexity).
- State space is continuous or non-discretizable without approximation.

## 5. Why It Works
A problem modeled as a Directed Acyclic Graph (DAG) of states allows evaluating each vertex exactly once in topological order. By computing states in topological order of dependencies, every state transition accesses finalized, guaranteed optimal sub-results.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force Recursion | Recompute identical subproblems across exponential branching tree | O(2^n) | O(n) stack |
| Dynamic Programming | Store subproblem results in a memo cache or DP table | O(n) to O(n^2) | O(n) table |

Dynamic programming trades table memory to eliminate redundant branches, transforming exponential runtimes into polynomial time.

## 7. Data Structures Used Here
- `int[]` / `int[][]`: Tabulation state arrays.
- `Map<State, Integer>` or array memo: Memoization caches for top-down recursion.

## 8. Core Template (Java)
```java
// Top-Down DP with Memoization skeleton
int solve(int n, int[] memo) {
    if (n <= 1) return n; // base case
    if (memo[n] != -1) return memo[n]; // return cached result
    memo[n] = solve(n - 1, memo) + solve(n - 2, memo); // transition
    return memo[n];
}

// Bottom-Up DP with Space Optimization skeleton
int climbStairs(int n) {
    if (n <= 2) return n;
    int prev2 = 1, prev1 = 2;
    for (int i = 3; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Greedy](../14-greedy/README.md) | [Tree Algorithms](../16-tree-algorithms/README.md)
