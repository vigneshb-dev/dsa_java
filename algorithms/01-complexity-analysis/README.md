# Complexity Analysis
> Asymptotic evaluation of algorithm runtime and memory consumption using Big-O, Big-Omega, and Big-Theta notations.

## 1. Overview
Complexity analysis evaluates how an algorithm's execution time and memory requirements scale as the input size $n$ tends toward infinity. It establishes platform-independent mathematical bounds to compare algorithm efficiency prior to implementation. By decoupling performance from hardware specifications, it allows engineers to predict feasibility and prevent timeouts under competitive and production constraints.

## 2. Input / Output
Typical inputs are algorithm implementations, recurrence relations, or loops along with input size $n$. The output is an asymptotic growth rate class (e.g. $O(n \log n)$ time, $O(1)$ auxiliary space).
*Example:* Input: nested loops iterating $n$ and $n/2$ times. Output: $O(n^2)$ time complexity.

## 3. Constraints
Typical operational thresholds for modern CPU execution (~$10^8$ operations per second limit):
- $n \le 10$: $O(n!)$ or $O(2^n \cdot n^2)$
- $n \le 20$: $O(2^n)$
- $n \le 500$: $O(n^3)$
- $n \le 5 \times 10^3$: $O(n^2)$
- $n \le 10^5$ to $10^6$: $O(n \log n)$ or $O(n)$
- $n \le 10^9$: $O(\sqrt{n})$ or $O(\log n)$
- $n > 10^9$: $O(1)$ or $O(\log n)$

## 4. Common Complexity Classes
- **$O(1)$ Constant:** Independent of $n$. Hash map lookup, array indexing, arithmetic operations.
- **$O(\log n)$ Logarithmic:** Problem size halved at each step. Binary search, Euclidean GCD.
- **$O(\sqrt{n})$ Square Root:** Primality test trial division, square-root block decomposition.
- **$O(n)$ Linear:** Single pass over input. Array traversal, sliding window, prefix sums.
- **$O(n \log n)$ Linearithmic:** Divide and conquer merges/partitions. Merge sort, quicksort average, heapsort.
- **$O(n^2)$ Quadratic:** Nested pairwise scans. Bubble sort, insertion sort, naive pair matching.
- **$O(n^3)$ Cubic:** Triple nested loops. Naive matrix multiplication, Floyd-Warshall shortest path.
- **$O(2^n)$ Exponential:** Subsets generation, naive Fibonacci recursion.
- **$O(n!)$ Factorial:** Permutations generation, brute-force Traveling Salesperson Problem.

## 5. How to Analyze Loops
1. **Single Loop:** Count iterations. If loop step is additive (`i += c`), it executes $O(n/c) = O(n)$ times. If step is multiplicative (`i *= 2`), it executes $O(\log_2 n)$ times.
2. **Nested Dependent Loops:** Sum the step counts across outer loop values. For `for (int i = 0; i < n; i++) for (int j = 0; j < i; j++)`, total steps = $\sum_{i=0}^{n-1} i = \frac{n(n-1)}{2} = O(n^2)$.
3. **Harmonic Series Loops:** For outer loop $i$ from 1 to $n$, inner loop step $j += i$: total steps = $\sum_{i=1}^n \frac{n}{i} = n \sum_{i=1}^n \frac{1}{i} = O(n \ln n)$ (e.g. Sieve of Eratosthenes variant).

## 6. How to Analyze Recursion
1. **Master Theorem:** For recurrences $T(n) = a T(n/b) + f(n)$:
   - If $f(n) = O(n^{\log_b a - \epsilon})$, then $T(n) = \Theta(n^{\log_b a})$.
   - If $f(n) = \Theta(n^{\log_b a} \log^k n)$, then $T(n) = \Theta(n^{\log_b a} \log^{k+1} n)$.
   - If $f(n) = \Omega(n^{\log_b a + \epsilon})$ and regularity holds, then $T(n) = \Theta(f(n))$.
2. **Recursion Tree Method:** Draw tree levels; compute work per level and total levels. E.g. Merge sort: $\log_2 n$ levels each doing $c \cdot n$ work $\implies O(n \log n)$.
3. **Substitution (Induction):** Guess asymptotic form and verify correctness via mathematical induction.

## 7. Amortized Analysis
Evaluates the average running time per operation over a worst-case sequence of operations:
- **Aggregate Method:** Total cost of $k$ operations is bounded by $T(k)$; amortized cost is $T(k) / k$. E.g. $n$ dynamic array pushes cost $O(n)$ total copies $\implies O(1)$ per push.
- **Accounting (Banker's) Method:** Assign fictitious charges (credits) to cheap operations to pay for rare expensive operations later.
- **Potential Method:** Define potential function $\Phi(D)$ over data structure state; amortized cost $\hat{c}_i = c_i + \Phi(D_i) - \Phi(D_{i-1})$.

## 8. Space Analysis
- **Auxiliary Space vs Total Space:** Total space includes input storage; auxiliary space measures only extra working memory allocated by the algorithm.
- **Recursion Call Stack:** Each recursive call frame occupies memory proportional to local variables. Call stack depth equals maximum tree/recursion depth ($O(h)$ for trees, up to $O(n)$ for skewed recursions).
- **In-Place Algorithms:** Algorithms that use $O(1)$ auxiliary memory beyond the input structure (e.g., in-place heapsort, two-pointer swaps).

## 16. When Should I Use This Algorithm?
- Evaluating algorithm feasibility before writing code against problem input constraints.
- Identifying computational bottlenecks in profiling hot paths.
- Selecting appropriate data structures based on operation frequency tradeoffs.
- Proving optimality lower bounds for theoretical algorithms.
- Comparing alternative architectural approaches during technical interviews.

## 17. When Should I NOT Use It?
- Benchmarking wall-clock execution for tiny $n$ where cache lines and low constant factors dominate over asymptotic curves (use micro-benchmarking).
- When hidden constant factors are massive (e.g. $O(n)$ with $c = 10^9$ versus $O(n^2)$ with $c = 1$).
- When memory hierarchy effects (L1/L2 cache misses) outweigh instruction count differences.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Complexity derivation practice problem |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | Start | [02 - Sorting](../02-sorting/README.md)
