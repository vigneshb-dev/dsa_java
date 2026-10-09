# Cyclic Sort
> In-place element placement for bounded integer ranges [1, n], solving missing and duplicate number problems in O(n) time and O(1) space.

## 1. Overview
Cyclic Sort is a specialized in-place sorting technique applicable when an array of size $n$ contains integers within a known bounded range—typically $[1, n]$ or $[0, n - 1]$. By continuously swapping each element into its target index (`nums[i] - 1`), it sorts the array or discovers missing and duplicate numbers in $O(n)$ time with strictly $O(1)$ auxiliary memory.

## 2. Input / Output
- Input: An unsorted array containing numbers in range $[1, n]$ with potential duplicates or missing entries (e.g. `nums = [3, 1, 5, 4, 2]`).
- Output: In-place sorted array or missing/duplicate identifier (e.g. sorted `[1, 2, 3, 4, 5]` or missing number).

## 3. Constraints
- Array length $n \le 10^7$.
- Array elements must be integers constrained within a range bounded by array dimensions (e.g., $[1, n]$, $[0, n]$, or $[1, n+1]$).
- Array must be mutable.

## 4. Brute-Force Approach
- Idea: Sort the array using general comparison sort ($O(n \log n)$) or store numbers in a `boolean[]` / `HashSet` to locate gaps.
- Pseudocode:
  ```java
  Set<Integer> set = new HashSet<>();
  for (int x : nums) set.add(x);
  for (int i = 1; i <= n; i++) if (!set.contains(i)) return i;
  ```
- Time: $O(n)$; Space: $O(n)$ auxiliary hash set memory.

## 5. Optimal Approach
- Idea: Iterate pointer $i$ from 0 to $n - 1$. While `nums[i]` is in bounds and not at its correct target index (`nums[i] - 1 != i`), swap `nums[i]` with the element at its target index, skipping if a duplicate already occupies that slot.
```java
// Reusable Cyclic Sort Skeleton (Numbers in range [1, n])
public class CyclicSortTemplate {
    public static void cyclicSort(int[] nums) {
        int i = 0;
        while (i < nums.length) {
            int correctIndex = nums[i] - 1; // Expected 0-based index for value nums[i]

            // Check if nums[i] is within valid range and not already at its correct slot
            if (correctIndex >= 0 && correctIndex < nums.length && nums[i] != nums[correctIndex]) {
                swap(nums, i, correctIndex); // Place element into its home index
            } else {
                i++; // Current slot is correct or duplicate; advance
            }
        }
    }

    // Finds the first missing positive integer in range [1, n]
    public static int findFirstMissingPositive(int[] nums) {
        cyclicSort(nums);
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != i + 1) {
                return i + 1; // First index where value does not match
            }
        }
        return nums.length + 1;
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Replaces general comparison sorting or hash set storage by using the array's own index positions as a natural hash map.
- **Cases skipped:** An element placed into its target index is never moved again.
- **Shortcuts / tricks used:** Target indexing trick: element $v$ belongs at index $v - 1$. Swapping immediately places at least one element into its final permanent resting place.
- **Time saved:** $O(n \log n) \to O(n)$; each swap correctly positions at least one misplaced number.
- **Space effect:** $O(n) \to O(1)$; zero extra arrays or hash tables allocated.
- **Trade-off:** Mutates the input array in-place.

## 6. Core Idea
Since the values are constrained to $[1, n]$, each number has a predetermined home index ($v \to v - 1$). Swapping each number directly to its home index reorganizes the array into identity mapping in linear time.

## 7. Pattern
- Pattern: Cyclic Sort / Index as Hash Key.
- Signals: "Numbers in range [1, n] or [0, n]", "find missing number", "find all duplicates in an array", "first missing positive", "set mismatch".

## 8. Data Structure Used
- Input array mutated in-place. Zero auxiliary memory structures ($O(1)$ space).

## 9. Invariant
At step $i$, all indices $j < i$ that do not contain missing/duplicate values hold their correct home elements (`nums[j] == j + 1`).

## 10. Dry Run
Sorting `[3, 4, -1, 1]` for First Missing Positive:
| Index $i$ | `nums[i]` | Target Index | Condition | Swap Action | Array State |
|---|---|---|---|---|---|
| 0 | 3 | 2 | $3 \ne nums[2] (-1)$ | Swap idx 0 and 2 | `[-1, 4, 3, 1]` |
| 0 | -1 | -2 | Out of bounds | Advance $i$ | `[-1, 4, 3, 1]` |
| 1 | 4 | 3 | $4 \ne nums[3] (1)$ | Swap idx 1 and 3 | `[-1, 1, 3, 4]` |
| 1 | 1 | 0 | $1 \ne nums[0] (-1)$ | Swap idx 1 and 0 | `[1, -1, 3, 4]` |
| 1 | -1 | -2 | Out of bounds | Advance $i$ | `[1, -1, 3, 4]` |
| 2 | 3 | 2 | Already at home | Advance $i$ | `[1, -1, 3, 4]` |
| 3 | 4 | 3 | Already at home | Advance $i$ | `[1, -1, 3, 4]` |

Scan: index 1 holds `-1 != 2` $\implies$ First missing positive is 2.

## 11. Edge Cases
- Duplicate values: guard check `nums[i] != nums[correctIndex]` avoids infinite swap loops when duplicates exist.
- Negative numbers or values $> n$: guard check `correctIndex >= 0 && correctIndex < nums.length` skips out-of-range elements.
- Array already sorted: passes through in $n$ increments without any swaps.

## 12. Correctness
Each swap places at least one previously misplaced element into its correct target index where it remains untouched. Because there are $n$ indices, at most $n$ successful swaps can occur before every element is placed or verified, guaranteeing termination and total correctness.

## 13. Time Complexity
- Best: $O(n)$ (already sorted; loop executes $n$ times with 0 swaps).
- Average / Worst: $O(n)$. Each swap puts at least one element into its correct position; at most $n - 1$ swaps can happen across all iterations. Total operations $\le 2n$.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ strictly constant in-place memory.

## 15. Can It Be Optimized?
Time $O(n)$ and auxiliary space $O(1)$ are mathematically optimal for finding missing/duplicate elements in an unsorted array.

## 16. When Should I Use This Algorithm?
- Problem states numbers are in range $[1, n]$ or $[0, n]$.
- Finding the single missing number or duplicate number in $O(1)$ space.
- Finding all numbers disappeared in an array.
- Finding the First Missing Positive integer (LeetCode 41).
- Set Mismatch (one duplicate, one missing).

## 17. When Should I NOT Use It?
- Array is read-only / immutable (use Floyd's cycle detection or bitwise XOR).
- Numbers span an unbounded arbitrary range (e.g. $[ -10^9, 10^9 ]$; use Sorting or Hashing).
- Floating-point or non-integer elements.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic cyclic sort problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [12 - Intervals](../12-intervals/README.md) | [14 - Greedy](../14-greedy/README.md)
