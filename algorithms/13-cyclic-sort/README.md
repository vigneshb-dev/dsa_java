# Cyclic Sort

> In-place linear sort placing numbers in range `[1..n]` into corresponding indices `[0..n-1]`.

## 1. Overview
Cyclic Sort is an in-place sorting pattern designed for arrays containing numbers in a known bounded range (typically `[1..n]` or `[0..n]`). It iterates through the array, swapping each misplaced number directly into its correct index (`val - 1`) until all elements reside at their designated positions. This achieves O(n) sorting with O(1) auxiliary space without requiring comparison sorting.

## 2. Time & Space Complexity
| Problem Variant | Target Range | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Missing Number | `[0..n]` | O(n) | O(1) |
| Find All Numbers Disappeared | `[1..n]` | O(n) | O(1) |
| Find Duplicate Number | `[1..n]` | O(n) | O(1) |
| First Missing Positive | `[1..n]` (arbitrary array) | O(n) | O(1) |

Each swap places at least one number into its permanent correct position. At most n swaps occur, guaranteeing total time <= 2*n = O(n).

## 3. When to Use
- Array contains integers in a known bounded range from `1` to `n` or `0` to `n`.
- Problem asks for missing numbers, duplicate numbers, or corrupted values in O(n) time and O(1) space.
- Finding the First Missing Positive integer in an unsorted array.
- Modifying the input array in-place is permitted.

## 4. When NOT to Use
- Numbers have an unbounded arbitrary range with values much larger than array length.
- Array contains floating point values or non-integer elements.
- Input array is immutable or in-place swapping is prohibited.

## 5. Why It Works
The bijection mapping value `x` to index `x - 1` partitions elements into permutation cycles. Placing an element at its target index either closes a cycle or extends it, ensuring that no element is swapped more than once into its destination.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Hash Set Lookup | Add all elements to `HashSet`; check presence of 1 to n | O(n) | O(n) |
| Cyclic Sort | Swap elements in-place to index `nums[i] - 1` | O(n) | O(1) |

Cyclic sort exploits index-as-hash-key placement to eliminate the O(n) auxiliary memory of hash tables.

## 7. Data Structures Used Here
- `int[]`: Direct in-place mutation of the primitive input array.

## 8. Core Template (Java)
```java
// Standard Cyclic Sort skeleton for numbers 1 to n
void cyclicSort(int[] nums) {
    int i = 0;
    while (i < nums.length) {
        int correctIdx = nums[i] - 1;
        if (nums[i] > 0 && nums[i] <= nums.length && nums[i] != nums[correctIdx]) {
            // Swap nums[i] to its correct position
            int temp = nums[i];
            nums[i] = nums[correctIdx];
            nums[correctIdx] = temp;
        } else {
            i++;
        }
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Intervals](../12-intervals/README.md) | [Greedy](../14-greedy/README.md)
