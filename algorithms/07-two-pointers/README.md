# Two Pointers

> Dual index navigation converging or scanning in tandem across linear sequences.

## 1. Overview
The Two Pointers technique coordinates two index markers across a linear data structure (typically an array or string) to search pairs or partition data. Pointers may start at opposite ends moving inward (converging), or start together moving in the same direction at varying rates. By taking advantage of sequence ordering, it typically reduces O(n^2) brute-force searches to single-pass O(n) operations.

## 2. Time & Space Complexity
| Pattern | Typical Input | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Opposite Ends (Converging) | Sorted array / palindrome string | O(n) | O(1) |
| Same Direction (Read/Write) | Unsorted array (in-place remove/partition) | O(n) | O(1) |
| Trapping Rain Water | Elevation height array | O(n) | O(1) |
| 3Sum (Sort + Two Pointers) | Array of integers | O(n^2) | O(log n) sort space |

Each pointer advances monotonically and visits each element at most once, guaranteeing strict O(n) overall time.

## 3. When to Use
- Finding pairs or triplets in a sorted array summing to a target value.
- Validating palindromes or reversing sequences in-place.
- Partitioning arrays in-place (e.g., Move Zeroes, Remove Duplicates, Dutch National Flag).
- Trapping rain water or container with most water geometric problems.

## 4. When NOT to Use
- Array is unsorted and sorting is prohibited or exceeds time budget.
- Data structure does not support bidirectional or indexed access (e.g., singly linked lists without fast/slow mechanics).
- Problem requires tracking non-monotonic contiguous segments (prefer sliding window or hash maps).

## 5. Why It Works
In a sorted array, if `nums[left] + nums[right] < target`, incrementing `left` is guaranteed to increase the sum, whereas decrementing `right` would decrease it. This monotonic certainty allows safely eliminating candidate pairs without exhaustive inspection.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Nested Loops) | Check every pair (i, j) for target sum | O(n^2) | O(1) |
| Optimized (Two Pointers) | Sort array and move left/right inward based on sum | O(n log n) | O(1) |

The sorted two-pointer invariant eliminates an entire nested loop, dropping runtime from quadratic to linear after sorting.

## 7. Data Structures Used Here
- `int[]` / `char[]`: Primitive indexed arrays.
- `String`: Accessed via `charAt()` or converted to `toCharArray()`.

## 8. Core Template (Java)
```java
// Converging two pointers on sorted array
int[] twoSumSorted(int[] numbers, int target) {
    int left = 0, right = numbers.length - 1;
    while (left < right) {
        int sum = numbers[left] + numbers[right];
        if (sum == target) {
            return new int[]{left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return new int[]{-1, -1};
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Divide and Conquer](../06-divide-and-conquer/README.md) | [Fast & Slow Pointers](../08-fast-slow-pointers/README.md)
