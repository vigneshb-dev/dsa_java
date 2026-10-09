# Divide and Conquer
> Breaking complex problems into independent subproblems, solving each recursively, and combining results.

## 1. Overview
Divide and Conquer breaks a problem into two or more smaller, non-overlapping independent subproblems of the same type, solves them recursively until simple enough to solve directly, and combines their solutions to form the global solution. It underpins algorithms such as Merge Sort, Quickselect, Karatsuba multiplication, and Closest Pair of Points.

## 2. Input / Output
- Input: Array, geometric point collection, or mathematical exponent base and power.
- Output: Combined global aggregate (e.g. sorted array, maximum subarray sum, or minimum Euclidean distance).
*Example:* Input: `nums = [-2, 1, -3, 4, -1, 2, 1, -5, 4]`; Output: Maximum subarray sum `6` (from `[4, -1, 2, 1]`).

## 3. Constraints
- Applies to $n$ up to $10^6$ for $O(n \log n)$ recurrences, or up to $n = 10^{18}$ for logarithmic matrix exponentiation.
- Subproblems must be strictly independent (solving one must not impact the state of another; otherwise use Dynamic Programming).

## 4. Brute-Force Approach
- Idea: Examine all $O(n^2)$ pairs or subsets directly without dividing the space.
- Pseudocode: Check every possible pair $(i, j)$ in $O(n^2)$ time.
- Time: $O(n^2)$; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Divide problem at midpoint, solve left and right halves recursively, then conquer across the midpoint boundary in linear or constant time.
```java
// Reusable Divide and Conquer Skeleton
public class DivideAndConquerTemplate {
    public static int solve(int[] nums, int left, int right) {
        // 1. Base Case: problem small enough to solve trivially
        if (left == right) {
            return nums[left];
        }

        // 2. Divide: find partition split
        int mid = left + (right - left) / 2;

        // 3. Conquer: solve independent subproblems recursively
        int leftAns = solve(nums, left, mid);
        int rightAns = solve(nums, mid + 1, right);

        // 4. Combine: compute cross-boundary solution
        int crossAns = combine(nums, left, mid, right);

        return Math.max(Math.max(leftAns, rightAns), crossAns);
    }

    private static int combine(int[] nums, int left, int mid, int right) {
        int leftSum = Integer.MIN_VALUE, sum = 0;
        for (int i = mid; i >= left; i--) {
            sum += nums[i];
            leftSum = Math.max(leftSum, sum);
        }
        int rightSum = Integer.MIN_VALUE; sum = 0;
        for (int i = mid + 1; i <= right; i++) {
            sum += nums[i];
            rightSum = Math.max(rightSum, sum);
        }
        return leftSum + rightSum;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes quadratic cross-comparisons between elements completely inside the left half and elements completely inside the right half.
- **Cases skipped:** Internal subproblem interactions are handled recursively in isolation; only boundary-crossing interactions are evaluated during combine.
- **Shortcuts / tricks used:** Halving input sizes yields recursion depth $\log_2 n$; Master Theorem bounds total cost to $O(n \log n)$ when combine is $O(n)$.
- **Time saved:** $O(n^2) \to O(n \log n)$ or $O(n^{\log_2 3})$ (Karatsuba).
- **Space effect:** Retains logarithmic $O(\log n)$ call-stack space.
- **Trade-off:** Incurs recursion function call overhead compared to flat iterative loops.

## 6. Core Idea
If a problem of size $n$ can be partitioned into disjoint subproblems of size $n/2$ whose individual solutions can be merged across the dividing seam in $O(n)$ time, the total work is bounded by $O(n \log n)$.

## 7. Pattern
- Pattern: Divide, Conquer, and Combine.
- Signals: "Maximum subarray sum", "merge k sorted lists", "count inversions", "closest pair of points", "fast matrix exponentiation".

## 8. Data Structure Used
- Recursion call stack (depth $O(\log n)$).
- Optional temporary merging arrays (e.g. `int[] temp`).

## 9. Invariant
The recursive call `solve(left, right)` returns the correct optimal solution for the sub-interval $[left, right]$ independently.

## 10. Dry Run
Maximum subarray over `[-2, 1, -3, 4]`:
| Level | Segment | Left Best | Right Best | Cross Best | Segment Answer |
|---|---|---|---|---|---|
| Leaf | `[0..0] (-2)` | -2 | - | - | -2 |
| Leaf | `[1..1] (1)` | 1 | - | - | 1 |
| Merge | `[0..1]` | -2 | 1 | -1 | 1 |
| Merge | `[2..3]` (`[-3, 4]`) | -3 | 4 | 1 | 4 |
| Global| `[0..3]` | 1 | 4 | $1 + (-3) + 4 = 2$ | 4 |

## 11. Edge Cases
- $n = 0$ or $n = 1$: handled cleanly by base case.
- Cross-boundary calculation edge cases (e.g. all negative numbers).
- Off-by-one mid indexing: always use `mid = left + (right - left) / 2` with partition `[left, mid]` and `[mid + 1, right]`.

## 12. Correctness
Follows from mathematical induction and the Law of Excluded Middle: Any optimal interval must either lie entirely in the left half, entirely in the right half, or cross the midpoint boundary. Since all three cases are computed and maximized, the true optimum is guaranteed.

## 13. Time Complexity
- Recurrence: $T(n) = 2T(n/2) + O(n)$. By Master Theorem ($a = 2, b = 2, f(n) = O(n)$), $T(n) = \Theta(n \log n)$.
- For fast exponentiation: $T(n) = T(n/2) + O(1) \implies O(\log n)$.

## 14. Space Complexity
- Auxiliary Space: $O(\log n)$ recursion stack frames for balanced partitions.

## 15. Can It Be Optimized?
Kadane's algorithm solves maximum subarray in $O(n)$ time and $O(1)$ space using Dynamic Programming, bypassing divide-and-conquer combine overhead.

## 16. When Should I Use This Algorithm?
- Subproblems are completely disjoint and do not overlap.
- Operations can be parallelized naturally across independent processor cores.
- Geometric algorithms like Closest Pair of Points in 2D ($O(n \log n)$ vs $O(n^2)$).
- Inversion counting in an array ($O(n \log n)$ modified merge sort).
- Fast Fourier Transform (FFT) polynomial multiplication.

## 17. When Should I NOT Use It?
- Subproblems overlap heavily (e.g. Fibonacci, shortest paths; use Dynamic Programming).
- A linear single-pass greedy or Kadane's scan exists ($O(n)$ beats $O(n \log n)$).
- Very small arrays where loop overhead is lower than function recursion overhead.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic divide-and-conquer problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [05 - Backtracking](../05-backtracking/README.md) | [07 - Two Pointers](../07-two-pointers/README.md)
