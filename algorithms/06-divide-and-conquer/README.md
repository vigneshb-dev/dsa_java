# Divide and Conquer

> Breaks problems into independent subproblems, solves recursively, and combines results.

## 1. Overview
Divide and Conquer is an algorithmic design paradigm that recursively breaks a problem down into two or more subproblems of the same or related type, until these become simple enough to be solved directly. The solutions to the subproblems are then combined to give a solution to the original problem. Classic applications include Merge Sort, Quick Sort, and Fast Exponentiation.

## 2. Time & Space Complexity
| Algorithm / Problem | Divide Cost | Conquer Subproblems | Combine Cost | Overall Time |
| :--- | :--- | :--- | :--- | :--- |
| Binary Search | O(1) | 1 of size n/2 | O(1) | O(log n) |
| Fast Exponentiation | O(1) | 1 of size n/2 | O(1) | O(log n) |
| Merge Sort | O(1) | 2 of size n/2 | O(n) | O(n log n) |
| Strassen's Matrix Mult | O(n^2) | 7 of size n/2 | O(n^2) | O(n^2.807) |

Dividing subproblems must yield non-overlapping subproblems; if subproblems overlap significantly, Dynamic Programming must be used instead.

## 3. When to Use
- Problem can be naturally partitioned into independent subproblems of the same structure.
- Combining solutions from subproblems is computationally cheaper than solving from scratch.
- Parallel or multi-threaded execution of independent subproblems is desirable.
- Tree problems where left and right subtrees can be computed independently.

## 4. When NOT to Use
- Subproblems overlap extensively (e.g., Fibonacci recurrence, where pure D&C causes exponential recomputation).
- The combine step is as expensive as solving the original problem directly.
- Iterative linear solutions exist with simpler code and less stack overhead.

## 5. Why It Works
Dividing problem size by a constant factor `b` limits the recursion tree depth to log_b(n). If the work done at each depth level is bounded by O(n), summing across log_b(n) levels yields efficient O(n log n) total execution.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Linear Power) | Multiply base x by itself n times | O(n) | O(1) |
| Optimized (Divide & Conquer Power) | Compute `x^(n/2)` once, square it, and multiply x if n is odd | O(log n) | O(log n) stack |

Fast exponentiation halves the exponent at every recursive step, dropping multiplications from linear to logarithmic.

## 7. Data Structures Used Here
- Auxiliary Arrays: Used in the combine step (e.g., merge buffer in Merge Sort).
- Call Stack: Stores execution context across logarithmic recursion levels.

## 8. Core Template (Java)
```java
// Fast Exponentiation via Divide and Conquer (x^n)
double myPow(double x, long n) {
    if (n < 0) {
        x = 1 / x;
        n = -n;
    }
    if (n == 0) return 1.0;
    double half = myPow(x, n / 2);
    if (n % 2 == 0) {
        return half * half;
    } else {
        return half * half * x;
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Backtracking](../05-backtracking/README.md) | [Two Pointers](../07-two-pointers/README.md)
