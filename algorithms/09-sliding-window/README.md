# Sliding Window
> Dynamic or fixed contiguous subarray/substring boundary tracking over sequential streams.

## 1. Overview
The Sliding Window pattern maintains a contiguous range $[left, right]$ over an array or string that expands rightward to consume elements and contracts from the left when constraints are breached. It reuses overlapping state between consecutive windows to convert $O(n^2)$ or $O(n^3)$ subarray scans into a single linear $O(n)$ pass.

## 2. Input / Output
- Input: An array or string and a constraint threshold (e.g. `s = "eceba"`, `k = 2` distinct characters).
- Output: An extremum length, count, or subsegment (e.g. longest substring length `3` for `"ece"`).

## 3. Constraints
- Scales to $n \le 10^7$ elements ($O(n)$ total time).
- Requires problem to query contiguous subarrays/substrings whose properties exhibit monotonic behavior with respect to window expansion/contraction (e.g., adding positive numbers always increases sum).

## 4. Brute-Force Approach
- Idea: Iterate through all $O(n^2)$ start and end indices $(i, j)$ and recompute the window property from scratch.
- Pseudocode:
  ```java
  int maxLen = 0;
  for (int i = 0; i < n; i++) {
      for (int j = i; j < n; j++) {
          if (isValid(s, i, j)) maxLen = Math.max(maxLen, j - i + 1);
      }
  }
  ```
- Time: $O(n^3)$ with naive recomputation, or $O(n^2)$ with incremental accumulation; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Advance `right` pointer to incorporate new element into state. While window condition is violated, advance `left` pointer to evict elements until validity is restored.
```java
// Reusable Variable-Size Sliding Window Skeleton
import java.util.*;

public class SlidingWindowTemplate {
    public static int longestSubarray(int[] nums, int k) {
        int left = 0;
        int maxLen = 0;
        int currentSum = 0; // Or frequency map / state tracker

        for (int right = 0; right < nums.length; right++) {
            // 1. Expand: include nums[right] into window state
            currentSum += nums[right];

            // 2. Shrink: contract from left while constraint violated
            while (currentSum > k && left <= right) {
                currentSum -= nums[left];
                left++;
            }

            // 3. Update Answer: window [left, right] is now strictly valid
            maxLen = Math.max(maxLen, right - left + 1);
        }

        return maxLen;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Completely eliminates re-evaluating the sum or frequency counts of overlapping elements shared between consecutive windows.
- **Cases skipped:** When window $[left, right]$ violates the condition, any larger window starting at $left$ that extends further right is also invalid (for non-negative numbers) and is skipped.
- **Shortcuts / tricks used:** Reuses running aggregate: adding `nums[right]` and subtracting `nums[left]` takes $O(1)$ rather than recalculating the entire range sum $O(k)$.
- **Time saved:** $O(n^2) \to O(n)$; both `left` and `right` pointers advance monotonically from $0$ to $n - 1$ at most once.
- **Space effect:** Retains $O(1)$ memory for numeric aggregates, or $O(\Sigma)$ bounded by alphabet size for character frequency maps.
- **Trade-off:** Requires state to be incrementally maintainable upon boundary additions and removals.

## 6. Core Idea
Rather than destroying and rebuilding the window state for every index pair, slide the window forward by adding the incoming element at `right` and discarding the outgoing element at `left`.

## 7. Pattern
- Pattern: Sliding Window (Fixed or Variable Size).
- Signals: "Contiguous subarray / substring", "longest substring with at most k distinct characters", "minimum size subarray sum", "maximum sum of subarray of size k", "anagrams in a string".

## 8. Data Structure Used
- Frequency table (`int[128]` for ASCII or `HashMap<Character, Integer>`) or primitive running sum accumulator.
- Two index pointer variables `left` and `right`.

## 9. Invariant
At the end of each iteration of the outer loop, the window `[left, right]` satisfies all problem constraints (or tracks the maximal valid window boundary).

## 10. Dry Run
Longest substring with sum $\le 5$ in `[2, 1, 3, 2]`:
| `right` | `nums[right]` | `currentSum` | Constraint Check | `left` | Window | Max Length |
|---|---|---|---|---|---|---|
| 0 | 2 | 2 | $2 \le 5$ (Valid) | 0 | `[2]` | 1 |
| 1 | 1 | 3 | $3 \le 5$ (Valid) | 0 | `[2, 1]` | 2 |
| 2 | 3 | 6 | $6 > 5 \implies$ shrink (sub 2) | 1 | `[1, 3]` (sum 4) | 2 |
| 3 | 2 | 6 | $6 > 5 \implies$ shrink (sub 1) | 2 | `[3, 2]` (sum 5) | 2 |

## 11. Edge Cases
- Empty string or array ($n = 0$): return 0.
- Target constraint cannot be satisfied by any window: returns 0 or initial default.
- Negative numbers present in sum queries: breaks monotonicity! (Sliding window fails; use Prefix Sum + HashMap instead).

## 12. Correctness
By monotonicity: For non-negative values, if window $[left, right]$ exceeds sum $S$, then for any $r' > right$, $[left, r']$ also exceeds $S$. Therefore, incrementing $left$ permanently is safe because no valid window starting at $left$ could extend further right.

## 13. Time Complexity
- Best / Average / Worst: $O(n)$. Although the `while` loop is nested, `left` increments at most $n$ times across the entire algorithm, yielding at most $2n$ total pointer operations.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ for integer counters; $O(\min(n, \Sigma))$ where $\Sigma$ is the alphabet size for character maps.

## 15. Can It Be Optimized?
Linear $O(n)$ time with single pass and $O(1)$ auxiliary space is asymptotically optimal.

## 16. When Should I Use This Algorithm?
- Problems specifying contiguous subarrays or substrings.
- Fixed window calculations (e.g. moving averages, max sum of size $k$).
- Variable window optimizing for maximum/minimum length under monotonic criteria.
- String permutation / anagram search in text.
- Longest substring without repeating characters.

## 17. When Should I NOT Use It?
- Array contains negative numbers with target sum constraints (use Prefix Sum with Hash Map).
- Problem asks for non-contiguous subsequences or subsets (use Dynamic Programming or Greedy).
- Tree or graph paths (use tree DP or DFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic sliding window problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [08 - Fast & Slow Pointers](../08-fast-slow-pointers/README.md) | [10 - Prefix Sum & Difference Array](../10-prefix-sum-difference-array/README.md)
