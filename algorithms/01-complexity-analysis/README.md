# Complexity Analysis

> Mathematical characterization of algorithmic runtime and memory usage as input scale grows.

## 1. Overview
Complexity Analysis quantifies how the execution time and memory requirements of an algorithm scale relative to input size n. Big-O notation establishes asymptotic upper bounds, ignoring hardware-specific constants and low-order terms. Mastering complexity analysis is foundational for predicting whether an algorithm will pass execution time constraints (typically ~10^8 operations per second in Java).

## 2. Time & Space Complexity
| Notation / Metric | Mathematical Meaning | Practical Interview Meaning |
| :--- | :--- | :--- |
| Big-O: O(f(n)) | Asymptotic upper bound (`T(n) <= c * f(n)`) | Worst-case or guaranteed runtime ceiling |
| Big-Omega: Omega(f(n)) | Asymptotic lower bound (`T(n) >= c * f(n)`) | Best-case lower bound |
| Big-Theta: Theta(f(n)) | Asymptotically tight bound | Exact order of growth (both upper and lower) |
| Auxiliary Space | Extra memory excluding input data | Space allocated by algorithm data structures |
| Space Complexity | Total memory including input and recursion call stack | Full runtime memory footprint |

In Java competitive programming and technical interviews, an algorithm must generally execute under 1.0 - 2.0 seconds, requiring total operations <= 10^8.

## 3. When to Use
- Evaluating whether an approach meets the problem's input size constraints (e.g., n = 10^5 mandates O(n) or O(n log n)).
- Comparing multiple candidate algorithms before writing implementation code.
- Identifying bottlenecks in nested loops or recursive function calls.
- Auditing auxiliary memory and call stack depth to prevent OutOfMemoryError and StackOverflowError.

## 4. When NOT to Use
- Input sizes are fixed and tiny (e.g., n <= 10, where asymptotic growth matters less than constant factors and clean code).
- Measuring hardware-specific CPU cycles or micro-benchmarking cache line misses in low-latency systems.
- Prematurely optimizing code before correctness is established.

## 5. Common Complexity Classes
The standard hierarchy of time complexity classes with typical constraint thresholds:

| Complexity | Growth Name | Typical Max Input Constraint (1s limit) | Example Problem Pattern |
| :--- | :--- | :--- | :--- |
| O(1) | Constant | Any size | Array index access, hash map lookup |
| O(log n) | Logarithmic | n <= 10^18 | Binary search, GCD Euclidean algorithm |
| O(sqrt(n)) | Square Root | n <= 10^12 | Primality testing, factorization |
| O(n) | Linear | n <= 10^7 - 10^8 | Single pass scan, two pointers, sliding window |
| O(n log n) | Linearithmic | n <= 10^5 - 10^6 | Merge sort, heap sort, divide and conquer |
| O(n^2) | Quadratic | n <= 5,000 | Nested loops, simple bubble/selection sort, 2D DP |
| O(n^3) | Cubic | n <= 500 | Floyd-Warshall, matrix multiplication |
| O(2^n) | Exponential | n <= 20 - 25 | Generating all subsets, recursion without memoization |
| O(n!) | Factorial | n <= 11 - 12 | Generating all permutations, traveling salesperson brute-force |

## 6. How to Analyze Code
1. **Loops and Iteration:**
   - Single loop from 1 to n: `O(n)`.
   - Nested loops where inner depends on outer (e.g., `for i from 1 to n; for j from i to n`): `n * (n + 1) / 2 = O(n^2)`.
   - Multiplicative loop increment (`i *= 2`): `O(log n)`.

2. **Recursion and Divide-and-Conquer:**
   - Master Theorem: `T(n) = a * T(n / b) + f(n)`. If `f(n) = O(n^c)` and `c = log_b(a)`, then `T(n) = O(n^c * log n)` (e.g., Merge Sort `2*T(n/2) + O(n) = O(n log n)`).
   - Recursion Tree: `Work = (number of nodes at level k) * (work per node at level k)` summed across all levels.

3. **Amortized Analysis:**
   - Evaluates the average cost of an operation over a sequence of operations.
   - Example: `ArrayList.add()` is amortized `O(1)`. Even though resizing costs `O(n)`, doubling capacity occurs only after n individual O(1) insertions.

4. **Space Complexity Breakdown:**
   - Auxiliary Data Structures: arrays, lists, maps allocated in memory.
   - Call Stack Frames: maximum recursion depth (e.g., tree DFS height `O(h)` frames).

## 7. Data Structures Used Here
- `ArrayList`: Dynamic array with amortized O(1) append and O(n) resize.
- `ArrayDeque`: Ring buffer with O(1) amortized queue/stack pushes and pops.
- `HashMap`: Bucket table with average O(1) and worst-case O(log n) operations.

## 8. Core Template (Java)
```java
// Demonstrating complexity analysis templates
// 1. O(log n) - Multiplicative iteration (Binary Division)
int countDivisions(int n) {
    int steps = 0;
    while (n > 1) {
        n /= 2;
        steps++;
    }
    return steps;
}

// 2. O(n) Time, O(1) Auxiliary Space - Single linear scan
int findMax(int[] nums) {
    int max = nums[0];
    for (int i = 1; i < nums.length; i++) {
        if (nums[i] > max) max = nums[i];
    }
    return max;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | Start | [Sorting](../02-sorting/README.md)
