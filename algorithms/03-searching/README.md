# Searching
> Locating target values or optimal boundaries within discrete collections or monotonic search spaces.

## 1. Overview
Searching algorithms determine the presence, location, or optimal boundary condition of a target value within a search space. While unsorted collections require scanning every element linearly, monotonic orderings permit logarithmic search via repeated interval bisection.

## 2. Input / Output
- Input: A sorted collection or monotonic predicate function $f(x)$, and target query value (e.g. `int[] arr = {1, 3, 5, 7, 9}`, `target = 5`).
- Output: The index or parameter satisfying the search condition (e.g. index `2`).

## 3. Constraints
- Linear search: applies to unsorted data for $n \le 10^7$.
- Binary search: applies to sorted arrays or monotonic answer spaces up to $n \le 10^{18}$ in $O(\log n)$ operations.
- Assumes the underlying search space exhibits monotonic behavior ($f(x)$ transitions from false to true or values increase monotonically).

## 4. Brute-Force Approach
- Idea: Scan every candidate element sequentially from start to end until matched or space exhausted.
- Pseudocode:
  ```java
  for (int i = 0; i < n; i++) {
      if (arr[i] == target) return i;
  }
  return -1;
  ```
- Time: $O(n)$ comparisons; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Binary search bisection repeatedly evaluates the midpoint and discards the half that cannot contain the target.
```java
// Reusable Binary Search Skeleton (Lower Bound / First True Template)
public class SearchingTemplate {
    // Finds first index where predicate is true, or arr[i] >= target
    public static int lowerBound(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        int ans = arr.length; // Default if not found

        while (left <= right) {
            int mid = left + (right - left) / 2; // Prevents 32-bit integer overflow
            if (arr[mid] >= target) {
                ans = mid;        // Candidate found; try searching further left
                right = mid - 1;
            } else {
                left = mid + 1;   // Value too small; must search right
            }
        }
        return ans;
    }

    // Binary Search on Answer: finds minimum x satisfying feasible(x)
    public static long binarySearchOnAnswer(long low, long high) {
        long ans = high;
        while (low <= high) {
            long mid = low + (high - low) / 2;
            if (feasible(mid)) {
                ans = mid;
                high = mid - 1; // Minimize answer
            } else {
                low = mid + 1;
            }
        }
        return ans;
    }

    private static boolean feasible(long val) {
        return true; // Problem-specific monotonicity check
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the need to inspect every candidate one by one.
- **Cases skipped:** Discards $n/2, n/4, \dots$ elements at each step; once midpoint comparison fails, the entire half is guaranteed invalid and skipped.
- **Shortcuts / tricks used:** Monotonicity allows a single comparison at the midpoint to deduce the validity of all elements on one side.
- **Time saved:** $O(n) \to O(\log n)$; for $n = 10^9$, reduces operations from 1,000,000,000 to ~30 comparisons.
- **Space effect:** Retains $O(1)$ memory by adjusting two boundary pointer variables.
- **Trade-off:** Requires the collection to be pre-sorted or the answer predicate to be monotonic.

## 6. Core Idea
Evaluate the midpoint of a monotonic domain. Because values are ordered, if the midpoint is smaller than target, every element to the left is also strictly smaller and can be discarded safely in a single operation.

## 7. Pattern
- Pattern: Binary Search / Bisection on Monotonic Space.
- Signals: "Sorted array", "find minimum maximum", "allocate minimum pages", "k-th smallest in matrix", "capacity to ship packages within D days".

## 8. Data Structure Used
- Two pointer variables (`left`, `right`) tracking candidate interval.
- Zero auxiliary memory structures ($O(1)$ space).

## 9. Invariant
The target (or optimal boundary) is strictly contained within the closed interval `[left, right]` (or tracked by `ans`).

## 10. Dry Run
Searching for target 7 in `[1, 3, 5, 7, 9, 11]`:
| Iteration | `left` | `right` | `mid` | `arr[mid]` | Condition | Next Range |
|---|---|---|---|---|---|---|
| 1 | 0 | 5 | 2 | 5 | $5 < 7 \implies$ left = mid + 1 | `[3, 5]` |
| 2 | 3 | 5 | 4 | 9 | $9 > 7 \implies$ right = mid - 1 | `[3, 3]` |
| 3 | 3 | 3 | 3 | 7 | $7 == 7 \implies$ found index 3 | Match! |

## 11. Edge Cases
- Target smaller than minimum or larger than maximum element: pointers terminate with `ans = length` or `ans = -1`.
- Single-element array: loop correctly executes once for `left == right`.
- Midpoint overflow: computing `mid = left + (right - left) / 2` avoids 32-bit integer overflow bug present in `(left + right) / 2`.

## 12. Correctness
By induction on search interval size: If target exists in $[L, R]$, comparing midpoint divides interval into $[L, mid - 1]$ or $[mid + 1, R]$. Since array is sorted, target cannot exist in the discarded half, maintaining search invariant until $L > R$.

## 13. Time Complexity
- Best: $O(1)$ (target located directly at first midpoint).
- Average: $O(\log n)$ via logarithmic interval bisection.
- Worst: $O(\log n)$ (requires $\approx \log_2 n$ bisections).

## 14. Space Complexity
- Auxiliary Space: $O(1)$ iterative; $O(\log n)$ if implemented recursively via call stack.

## 15. Can It Be Optimized?
Information-theoretic lower bound for searching an unsorted array is $\Omega(n)$ and for comparison searching in sorted array is $\Omega(\log n)$. Interpolation search achieves $O(\log \log n)$ on strictly uniformly distributed data.

## 16. When Should I Use This Algorithm?
- Input array is sorted and element lookups are requested.
- Finding first or last occurrence of a duplicate key.
- Optimization problems minimizing the maximum or maximizing the minimum (Binary Search on Answer).
- Finding square root or integer inverse functions.
- Peak element discovery in bitonic or unimodal arrays.

## 17. When Should I NOT Use It?
- Unsorted data where one-off search is needed (sorting takes $O(n \log n)$, which is slower than $O(n)$ linear scan).
- Linked lists (lacks $O(1)$ random midpoint indexing).
- Dynamic collections with frequent insertions and deletions (use `TreeSet` or `HashMap`).

---

## Comparison Table
| Algorithm | Time (Best / Avg / Worst) | Space | Monotonicity Required | Best Use |
|---|---|---|---|---|
| Linear Search | $O(1) / O(n) / O(n)$ | $O(1)$ | No | Small or unsorted arrays |
| Binary Search (Exact) | $O(1) / O(\log n) / O(\log n)$ | $O(1)$ | Yes | Sorted array item lookup |
| Lower / Upper Bound | $O(1) / O(\log n) / O(\log n)$ | $O(1)$ | Yes | Duplicate handling, insertion points, range counts |
| Binary Search on Answer | $O(1) / O(C \log K) / O(C \log K)$ | $O(1)$ | Yes (Predicate) | Min-max optimization, capacity planning |
| Ternary Search | $O(1) / O(\log_3 n) / O(\log_3 n)$ | $O(1)$ | Unimodal (Peak/Valley) | Finding extrema of convex/concave functions |

---

### Algorithm: Linear Search
- **Input / Output:** Unsorted array and target $\to$ Index or -1. E.g., `[7, 2, 4]`, target 2 $\to$ 1.
- **Constraints:** $n \le 10^7$, arbitrary unsorted data.
- **Brute Force:** Scan from 0 to $n - 1$ ($O(n)$ time, $O(1)$ space).
- **Optimal Approach:** Same as brute-force; optimal without prior knowledge.
- **How It Reduces Time/Space:** No reductions possible without sorted structure.
- **Core Idea:** Inspect elements one by one until match is found.
- **Pattern:** Sequential traversal.
- **Data Structure Used:** Raw array.
- **Invariant:** Target has not appeared in scanned prefix `arr[0..i-1]`.
- **Dry Run:** Scans `[7, 2]` $\implies$ match at index 1.
- **Edge Cases:** Target absent, empty array.
- **Correctness:** By exhaustion: checks all elements before declaring absent.
- **Time Complexity:** Best: $O(1)$, Avg/Worst: $O(n)$.
- **Space Complexity:** $O(1)$ auxiliary memory.
- **Can It Be Optimized:** Cannot be optimized without sorting or hashing.
- **When to Use:** Unsorted arrays with one single query.
- **When NOT to Use:** Multiple queries or large sorted arrays (use Binary Search or Hash Table).

### Algorithm: Binary Search on Answer
- **Input / Output:** Search range `[low, high]` and boolean predicate $f(x)$ $\to$ optimal value $x^*$.
- **Constraints:** Monotonic function $f(x)$ (transitions `F, F, ..., F, T, T, ...`); range up to $10^{18}$.
- **Brute Force:** Test all candidate answers sequentially from low to high ($O(K \cdot C)$ where $C$ is check cost).
- **Optimal Approach:** Binary search on candidate answer range `[low, high]` testing feasibility of `mid`.
- **How It Reduces Time/Space:** Skips testing 99.9% of candidate values; reduces candidate tests from $K$ to $\log_2 K$.
- **Core Idea:** Convert optimization problem "find minimum $x$" into decision problem "is $x$ feasible?".
- **Pattern:** Binary search on answer / monotonic decision function.
- **Data Structure Used:** Two variables `low`, `high`.
- **Invariant:** Feasible boundary is maintained within `[low, high]`.
- **Dry Run:** Capacity range `[1..10]`; mid 5 is feasible $\implies$ search `[1..4]`.
- **Edge Cases:** Impossible constraints, overflow in `high` (use `long`).
- **Correctness:** Monotonicity guarantees that if `mid` is valid, all values $> mid$ are valid.
- **Time Complexity:** $O(C \cdot \log(\text{high} - \text{low}))$ where $C$ is cost of verification check.
- **Space Complexity:** $O(1)$ auxiliary space.
- **Can It Be Optimized:** Optimal bisection.
- **When to Use:** Problems asking for "minimum maximum", "maximum minimum", or capacity scheduling.
- **When NOT to Use:** Non-monotonic feasibility functions (use DP or backtracking).

### Algorithm: Ternary Search
- **Input / Output:** Unimodal array or continuous range $\to$ Peak or trough coordinate.
- **Constraints:** Domain is strictly strictly increasing then decreasing (or vice versa).
- **Brute Force:** Linear scan across entire domain ($O(n)$).
- **Optimal Approach:** Trisect range using `m1 = l + (r - l)/3` and `m2 = r - (r - l)/3`; discard outer third.
- **How It Reduces Time/Space:** Reduces search space by $1/3$ each step, taking $2 \log_{1.5} n$ iterations.
- **Core Idea:** Compare two internal probe points to discard the sub-interval that cannot contain the extremum.
- **Pattern:** Ternary bisection on unimodal functions.
- **Data Structure Used:** Boundary pointers `l`, `r`.
- **Invariant:** Extremum point lies within `[l, r]`.
- **Dry Run:** Parabola peak in `[0, 9]`; probe at 3 and 6; discard `[0, 3]`.
- **Edge Cases:** Flat plateaus break unimodality.
- **Correctness:** Proved by unimodal geometry: if $f(m_1) < f(m_2)$, peak cannot lie in $[l, m_1]$.
- **Time Complexity:** $O(\log_{1.5} n)$ iterations.
- **Space Complexity:** $O(1)$ auxiliary space.
- **Can It Be Optimized:** Golden section search reduces evaluations per iteration.
- **When to Use:** Finding maximum or minimum of strictly unimodal continuous functions or bitonic arrays.
- **When NOT to Use:** Arbitrary multi-modal functions with local extrema (use gradient descent or grid search).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic binary search problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [02 - Sorting](../02-sorting/README.md) | [04 - Recursion](../04-recursion/README.md)
