# Two Pointers
> Coordinated multi-pointer traversal over arrays or sequences to evaluate pairs, partitions, or palindromes in linear time.

## 1. Overview
The Two Pointers pattern coordinates two integer index variables traversing a linear sequence either inward toward each other (converging pointers) or simultaneously in the same direction at differing rates. It leverages sorted order or partition predicates to eliminate an entire nested loop, achieving $O(n)$ time and $O(1)$ space.

## 2. Input / Output
- Input: Array or string, often sorted or partitioned, with a target property (e.g. `nums = [1, 2, 4, 7, 11]`, `target = 9`).
- Output: Indices, pairs, container volume, or boolean validity (e.g. `[2, 7]` matching sum `9`).

## 3. Constraints
- Scales to $n \le 10^7$ because traversal requires only a single pass ($O(n)$ time).
- Input must be sorted for target sum problems, or order must preserve the monotonic property governing pointer movement.

## 4. Brute-Force Approach
- Idea: Check every possible pair $(i, j)$ using two nested loops.
- Pseudocode:
  ```java
  for (int i = 0; i < n; i++) {
      for (int j = i + 1; j < n; j++) {
          if (nums[i] + nums[j] == target) return new int[]{i, j};
      }
  }
  ```
- Time: $O(n^2)$; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Place `left` at start (0) and `right` at end ($n - 1$). Compute sum: if sum < target, advance `left++`; if sum > target, decrement `right--`.
```java
// Reusable Inward-Converging Two Pointers Skeleton (Sorted Pair Sum Template)
public class TwoPointersTemplate {
    public static int[] twoSumSorted(int[] numbers, int target) {
        int left = 0;
        int right = numbers.length - 1;

        while (left < right) {
            int currentSum = numbers[left] + numbers[right];

            if (currentSum == target) {
                return new int[]{left, right}; // Found target pair
            } else if (currentSum < target) {
                left++;  // Sum too small; increase left pointer to larger value
            } else {
                right--; // Sum too large; decrease right pointer to smaller value
            }
        }
        return new int[]{-1, -1}; // Not found
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Eliminates redundant pair tests. If `nums[left] + nums[right] > target`, then because the array is sorted, pairing `nums[right]` with ANY element $> left$ would also be $> target$, safely skipping an entire row of pairs.
- **Cases skipped:** Each pointer step discards an entire candidate element permanently without testing it against remaining elements.
- **Shortcuts / tricks used:** Sorted order monotonicity allows the directional pointer movement to guarantee that the discarded element cannot participate in any valid solution.
- **Time saved:** $O(n^2) \to O(n)$; reduces $\approx n^2/2$ pair evaluations to at most $n$ total pointer increments.
- **Space effect:** Retains $O(1)$ memory by operating directly on the input array in-place without auxiliary hash tables.
- **Trade-off:** Requires the input array to be sorted upfront ($O(n \log n)$ if not already sorted).

## 6. Core Idea
In a sorted sequence, moving the left pointer rightward strictly increases the sum, while moving the right pointer leftward strictly decreases the sum. At each step, comparing the current sum to the target dictates the only productive direction to adjust.

## 7. Pattern
- Pattern: Converging Two Pointers / Opposite-Direction Traversal.
- Signals: "Sorted array", "pair with target sum", "3Sum / 4Sum", "container with most water", "trapping rain water", "valid palindrome", "reverse in-place".

## 8. Data Structure Used
- Two primitive integer variables (`left`, `right`) holding array indices.
- No auxiliary memory allocation ($O(1)$ space).

## 9. Invariant
At every step, the target pair (if one exists) must lie within the closed index interval `[left, right]`; all elements outside this window have been proven invalid.

## 10. Dry Run
Target sum 9 on `[1, 2, 4, 7, 11]`:
| Step | `left` (val) | `right` (val) | `currentSum` | Comparison vs 9 | Action | Next Window |
|---|---|---|---|---|---|---|
| 1 | 0 (1) | 4 (11) | 12 | $12 > 9$ | `right--` | `[0, 3]` |
| 2 | 0 (1) | 3 (7) | 8 | $8 < 9$ | `left++` | `[1, 3]` |
| 3 | 1 (2) | 3 (7) | 9 | $9 == 9$ | Found target | Return `[1, 3]` |

## 11. Edge Cases
- No valid pair exists: loop terminates safely when `left >= right`.
- Duplicate elements in 3Sum/4Sum: must skip identical values (`while (nums[i] == nums[i+1]) i++`) to avoid duplicate triplets.
- Integer overflow on sum calculation: cast to `long` if values approach `Integer.MAX_VALUE`.

## 12. Correctness
By deduction: If `nums[left] + nums[right] > target`, then since `nums` is sorted, `nums[k] + nums[right] > target` for all $k \ge left$. Thus `nums[right]` cannot pair with any remaining candidate and is safely eliminated. Symmetric logic holds when sum < target.

## 13. Time Complexity
- Best: $O(1)$ (target pair found on first probe).
- Average / Worst: $O(n)$ because each step advances `left` or decrements `right`, completing in at most $n$ iterations.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ strictly in-place.

## 15. Can It Be Optimized?
Time complexity is already $O(n)$ with $O(1)$ auxiliary space, which is optimal for scanning an array.

## 16. When Should I Use This Algorithm?
- Searching for pairs with a given target sum in a sorted array.
- Finding container with most water or trapping rainwater.
- Validating palindromes or reversing arrays in-place.
- Dutch National Flag / 3-way partitioning (`0, 1, 2` sorting).
- Merging two pre-sorted arrays without extra buffers.

## 17. When Should I NOT Use It?
- Unsorted array where element indices must be preserved and sorting is not allowed (use a `HashMap` for $O(n)$ Two Sum).
- Searching for subarrays rather than pairs (use Sliding Window or Prefix Sums).
- Linked lists without bidirectional pointers (use Fast & Slow Pointers).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic two pointers problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [06 - Divide and Conquer](../06-divide-and-conquer/README.md) | [08 - Fast & Slow Pointers](../08-fast-slow-pointers/README.md)
