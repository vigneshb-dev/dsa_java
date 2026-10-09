# Prefix Sum & Difference Array
> Precomputing cumulative sums or boundary differentials to answer static range queries or execute range updates in O(1) time.

## 1. Overview
Prefix Sum precomputes running cumulative sums across an array, enabling any contiguous subarray sum query $[L, R]$ to be answered in constant $O(1)$ time. Dually, the Difference Array records boundary increments at $L$ and decrements at $R + 1$, allowing multiple range updates $[L, R, +v]$ to execute in $O(1)$ time followed by a single reconstruction sweep.

## 2. Input / Output
- Input: Array `nums` and a list of $Q$ range sum queries $[L, R]$ (or range update tuples $[L, R, val]$).
- Output: Exact sum values for each range in $O(1)$ (or updated array after all $Q$ operations).
*Example:* `nums = [2, 4, 1, 3]`; query $[1, 3]$ $\to$ sum = $4 + 1 + 3 = 8$.

## 3. Constraints
- Array size $n \le 10^7$, number of queries $Q \le 10^7$.
- Elements can be positive, negative, or zero (unlike sliding window, prefix sum natively handles negatives).
- Sum accumulator values can exceed 32-bit integer limits (requires `long[]` prefix arrays).

## 4. Brute-Force Approach
- Idea: For every query $[L, R]$, run a `for` loop from $L$ to $R$ summing values.
- Pseudocode:
  ```java
  for (int q = 0; q < Q; q++) {
      int sum = 0;
      for (int i = L; i <= R; i++) sum += nums[i];
      results[q] = sum;
  }
  ```
- Time: $O(Q \cdot n)$ total time; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Precompute 1-indexed `prefix[i] = prefix[i - 1] + nums[i - 1]`. Any query sum is `prefix[R + 1] - prefix[L]` in $O(1)$. For range updates, modify endpoints in `diff` array and compute prefix sums at the end.
```java
// Reusable Prefix Sum and Difference Array Template
public class PrefixSumTemplate {
    // Prefix Sum: O(n) preprocessing, O(1) range sum queries
    public static long[] buildPrefixSum(int[] nums) {
        long[] prefix = new long[nums.length + 1];
        for (int i = 0; i < nums.length; i++) {
            prefix[i + 1] = prefix[i] + nums[i];
        }
        return prefix;
    }

    public static long queryRangeSum(long[] prefix, int left, int right) {
        return prefix[right + 1] - prefix[left];
    }

    // Difference Array: O(1) range updates, O(n) final reconstruction
    public static class DifferenceArray {
        private final int[] diff;

        public DifferenceArray(int n) {
            this.diff = new int[n + 1]; // +1 for 1-past-the-end marking
        }

        public void addRange(int l, int r, int val) {
            diff[l] += val;
            if (r + 1 < diff.length) diff[r + 1] -= val;
        }

        public int[] reconstruct(int n) {
            int[] result = new int[n];
            int running = 0;
            for (int i = 0; i < n; i++) {
                running += diff[i];
                result[i] = running;
            }
            return result;
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes repeated redundant summations over the same index intervals across multiple queries.
- **Cases skipped:** Bypasses element-by-element iteration entirely during query time.
- **Shortcuts / tricks used:** Inverse operation trick: range sum $\sum_{i=L}^R nums[i] = (\sum_{i=0}^R nums[i]) - (\sum_{i=0}^{L-1} nums[i])$.
- **Time saved:** $O(Q \cdot n) \to O(n + Q)$; for $10^5$ queries on $10^5$ elements, reduces operations from $10^{10}$ to $2 \times 10^5$.
- **Space effect:** Allocates $O(n)$ space for the prefix or difference buffer.
- **Trade-off:** Precomputation space $O(n)$; static (modifying the underlying array requires recomputing the prefix sum in $O(n)$).

## 6. Core Idea
Precompute cumulative progress once up front so any subsegment answer can be extracted by subtracting two boundary checkpoints. Differentially, marking $+val$ at start and $-val$ at end propagates the delta across the interval via a final prefix sweep.

## 7. Pattern
- Pattern: Cumulative Prefix Sum / Difference Array / 2D Integral Image.
- Signals: "Subarray sum equals k", "range sum queries on immutable array", "corporate flight bookings (range additions)", "continuous subarray sum divisible by k", "maximum size rectangle in grid".

## 8. Data Structure Used
- Flat primitive array `long[n + 1]` for prefix sums.
- Flat primitive array `int[n + 1]` for difference markers.

## 9. Invariant
At index $i$, `prefix[i]` holds the exact sum of all elements in the half-open prefix interval `nums[0..i-1]`.

## 10. Dry Run
Array `nums = [3, 1, 4, 2]`:
| Index $i$ | `nums[i]` | `prefix[i]` (before) | `prefix[i+1]` (after) | Query `[1, 2]` ($1 + 4 = 5$) |
|---|---|---|---|---|
| 0 | 3 | 0 | 3 | `prefix[3] - prefix[1]` |
| 1 | 1 | 3 | 4 | $= 8 - 3$ |
| 2 | 4 | 4 | 8 | $= 5$ (Match!) |
| 3 | 2 | 8 | 10 | - |

## 11. Edge Cases
- 0-indexed offset errors: using an $(n + 1)$-sized prefix array where `prefix[0] = 0` eliminates bounds-checking when $L = 0$.
- Numeric overflow: using `long[]` prevents 32-bit integer wraparound when array contains large numbers.
- Range update boundaries: bounds check `if (r + 1 < diff.length)` prevents `ArrayIndexOutOfBoundsException`.

## 12. Correctness
By algebraic telescoping: $\sum_{i=L}^R A[i] = \sum_{i=0}^R A[i] - \sum_{i=0}^{L-1} A[i] = P[R + 1] - P[L]$. For difference arrays, $\sum_{k=0}^i (D[k]) = \sum_{k=0}^i (A[k] - A[k-1]) = A[i] - A[-1] = A[i]$.

## 13. Time Complexity
- Preprocessing: $O(n)$ one-time linear pass.
- Query / Range Update: $O(1)$ constant time per query.
- Total for $Q$ queries: $O(n + Q)$.

## 14. Space Complexity
- Auxiliary Space: $O(n)$ memory for `prefix` or `diff` array.

## 15. Can It Be Optimized?
Query time $O(1)$ is already mathematically optimal. If the underlying array undergoes dynamic modifications intermixed with queries, upgrade to a Fenwick Tree or Segment Tree for $O(\log n)$ updates and queries.

## 16. When Should I Use This Algorithm?
- Answering multiple range sum queries on an immutable array.
- Finding the count of subarrays with sum equal to $K$ (pair with `HashMap<Long, Integer>`).
- Executing multiple batch range addition operations offline.
- Finding subarrays whose sums are divisible by $K$ (using modulo prefixes).
- 2D grid range sum queries (2D Prefix Sum / Integral Image).

## 17. When Should I NOT Use It?
- Array is updated dynamically between queries (use Fenwick Tree or Segment Tree).
- Finding range minimum or maximum queries (min/max does not have an inverse subtraction operator; use Sparse Table or Segment Tree).
- Single query where preprocessing $O(n)$ exceeds the cost of a direct scan.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic prefix sum problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [09 - Sliding Window](../09-sliding-window/README.md) | [11 - Monotonic Stack & Queue](../11-monotonic-stack-queue/README.md)
