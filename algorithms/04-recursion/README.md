# Recursion

> Methodological problem solving where functions solve self-similar subproblems with base cases.

## 1. Overview
Recursion is a computational paradigm where a method solves a problem by calling itself with smaller instances of the same input. Every recursive function requires one or more base cases to terminate without infinite loops, and a recursive step that reduces the state. Each recursive call creates an execution frame on the call stack, consuming O(depth) auxiliary memory.

## 2. Time & Space Complexity
| Pattern / Variant | Recurrence | Time Complexity | Call Stack Space |
| :--- | :--- | :--- | :--- |
| Linear Recursion (e.g. Factorial) | T(n) = T(n-1) + O(1) | O(n) | O(n) |
| Binary Tree Traversal | T(n) = 2*T(n/2) + O(1) | O(n) | O(h) where h is height |
| Divide & Conquer (Merge Sort) | T(n) = 2*T(n/2) + O(n) | O(n log n) | O(log n) |
| Branching Recursion (Fibonacci) | T(n) = T(n-1) + T(n-2) | O(2^n) | O(n) |

Un-memoized branching recursion results in exponential O(2^n) time due to repeated computation of overlapping subproblems.

## 3. When to Use
- Data structure has an inherently recursive definition (trees, graphs, nested JSON/lists).
- Divide-and-conquer decompositions (e.g., merge sort, fast exponentiation).
- Mathematical recurrences (Tower of Hanoi, permutations, combinations).
- Problems requiring exploring all paths or configurations (backtracking foundations).

## 4. When NOT to Use
- Simple iterative loops exist with O(1) space (e.g., simple array iteration).
- Recursion depth can exceed call stack limits (~5,000 - 10,000 frames in Java), risking `StackOverflowError`.
- Subproblems overlap heavily without memoization (use Dynamic Programming instead).

## 5. Why It Works
Recursion leverages mathematical induction: if the base case is correct, and assuming subproblems of size < n are solved correctly, then combining them yields a correct solution for size n. The JVM call stack automatically tracks local variables and execution resumption points for every call.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Naive Recursive Fibonacci | Redundantly recompute subproblems `fib(n-1) + fib(n-2)` | O(2^n) | O(n) stack |
| Memoized / Iterative Fibonacci | Cache or carry forward previous two values | O(n) | O(1) space |

Caching previously computed states trades table memory to collapse exponential recursion trees into linear runtime.

## 7. Data Structures Used Here
- JVM Call Stack: Implicit system stack maintaining activation records.
- `ArrayDeque`: Explicit stack data structure when simulating recursion iteratively.

## 8. Core Template (Java)
```java
// Standard recursion skeleton: Base Case + Recursive Step
int solve(int n) {
    // 1. Base case(s)
    if (n <= 1) {
        return n;
    }
    // 2. Recursive step (reduce subproblem)
    int subproblem = solve(n - 1);
    // 3. Combine
    return subproblem + n;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Searching](../03-searching/README.md) | [Backtracking](../05-backtracking/README.md)
