# Recursion
> Solving problems by decomposing them into smaller self-similar instances of the same problem.

## 1. Overview
Recursion is an algorithmic paradigm where a method solves a problem by calling itself with reduced input parameters until a terminal base case is reached. It naturally expresses hierarchical traversals, divide-and-conquer decompositions, and mathematical inductive relations.

## 2. Input / Output
- Input: Problem state parameters (e.g. integer $n$ or tree root node).
- Output: Aggregate result or decomposed state (e.g. factorial $n!$ or height of binary tree).
*Example:* Input: $n = 4$; Output: $24$ ($4 \times 3 \times 2 \times 1$).

## 3. Constraints
- Call stack depth in standard JVM is limited to $\approx 5,000 - 10,000$ frames before throwing `StackOverflowError`.
- For linear recursion depth, $n \le 5,000$; for balanced divide-and-conquer trees ($n/2$), inputs up to $n = 10^9$ are safe since $\log_2 n \le 30$.

## 4. Brute-Force Approach
- Idea: Un-memoized naive recursion recomputes overlapping subproblems exponentially (e.g. naive Fibonacci).
- Pseudocode:
  ```java
  int fib(int n) {
      if (n <= 1) return n;
      return fib(n - 1) + fib(n - 2); // Explodes to O(2^n) calls
  }
  ```
- Time: $O(2^n)$; Space: $O(n)$ call stack depth.

## 5. Optimal Approach
- Idea: Clear base cases, reduced subproblem steps, and tail-recursion or memoization to eliminate overlapping work.
```java
// Reusable Recursive Tree/Divide-and-Conquer Skeleton
public class RecursionTemplate {
    public static int solve(int n) {
        // 1. Base Case: terminate recursion immediately
        if (n <= 1) {
            return n;
        }

        // 2. Divide / Subproblem Call
        int leftResult = solve(n - 1);

        // 3. Combine / Conquer Step
        return combine(n, leftResult);
    }

    private static int combine(int state, int subResult) {
        return state + subResult;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Redundant branch re-evaluations are eliminated by memoization or linear recurrence formulation.
- **Cases skipped:** Base cases halt execution without descending into negative or invalid states.
- **Shortcuts / tricks used:** Passing accumulator states (tail recursion) or caching results in a memo table.
- **Time saved:** $O(2^n) \to O(n)$ when overlapping branches are pruned.
- **Space effect:** Retains call stack bounded by recursion depth $O(d)$.
- **Trade-off:** Stack frames incur memory overhead relative to iterative loops.

## 6. Core Idea
Every valid recursive function consists of two essential pillars: one or more terminating base cases that return without recursing, and a recurrence relation that strictly progresses parameters toward the base cases.

## 7. Pattern
- Pattern: Inductive State Reduction / Tree Traversal.
- Signals: "Tree / graph structure", "nested expressions", "towers of hanoi", "combinations / permutations", "subproblems self-similar to original".

## 8. Data Structure Used
- JVM Call Stack: automatically allocates stack frames storing local variables and return program counters.

## 9. Invariant
At depth $k$, all preconditions for problem state $S_k$ hold, and each recursive step receives a strictly smaller parameter that converges toward the base case.

## 10. Dry Run
Computing `factorial(3)`:
| Frame | Call | Base Condition | Return Value | Stack Action |
|---|---|---|---|---|
| 1 | `fact(3)` | false | Waits for `fact(2)` | Push frame 1 |
| 2 | `fact(2)` | false | Waits for `fact(1)` | Push frame 2 |
| 3 | `fact(1)` | true ($n \le 1$) | Returns 1 | Pop frame 3 |
| 2 | Resumes `fact(2)` | - | $2 \times 1 = 2$ | Pop frame 2 |
| 1 | Resumes `fact(3)` | - | $3 \times 2 = 6$ | Pop frame 1 |

## 11. Edge Cases
- Missing base case: results in infinite recursion and `StackOverflowError`.
- Non-progressing arguments (e.g. `solve(n)` calling `solve(n)`): causes infinite loop.
- Negative numbers or overflow on large inputs (e.g. integer overflow in factorials).

## 12. Correctness
Proved by mathematical induction: If the base case $P(0)$ is correct, and assuming $P(k)$ correctly solves the subproblem allows $P(k+1)$ to compute the correct result, then $P(n)$ is correct for all $n \ge 0$.

## 13. Time Complexity
- Best / Average / Worst: Evaluated via Master Theorem or recursion tree: $T(n) = a T(n/b) + f(n)$. For single reduction $T(n) = T(n - 1) + O(1)$, complexity is $O(n)$.

## 14. Space Complexity
- Auxiliary Space: $O(d)$ where $d$ is the maximum recursion call-stack depth ($O(n)$ linear, $O(\log n)$ balanced tree).

## 15. Can It Be Optimized?
Can be converted to an iterative loop with an explicit `ArrayDeque` stack to avoid JVM stack limits, or transformed to iterative Dynamic Programming.

## 16. When Should I Use This Algorithm?
- Hierarchical tree and graph traversals (DFS, AST evaluation).
- Divide and conquer problems (Merge sort, Quicksort).
- Naturally inductive mathematical formulas.
- Backtracking search spaces where call stack manages backtrack states.
- Reversing singly linked lists or printing linked sequences in reverse.

## 17. When Should I NOT Use It?
- Deep linear iterations where $n > 5,000$ (risk of `StackOverflowError`; use a `while` loop).
- Un-memoized overlapping subproblems (use DP / tabulation).
- Performance-critical embedded systems with tight stack memory limits.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic recursion practice problem |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [03 - Searching](../03-searching/README.md) | [05 - Backtracking](../05-backtracking/README.md)
