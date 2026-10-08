# Searching

> Techniques for locating targets or monotonic boundaries in collections and answer spaces.

## 1. Overview
Searching locates the presence, index, or boundary condition of an element within a search domain. Linear search scans unsorted data in O(n) time, whereas Binary Search eliminates half the remaining search space at every step in O(log n) time. Binary search generalizes beyond sorted arrays to any monotonic predicate function ('Binary Search on Answer').

## 2. Time & Space Complexity
| Variant | Best Time | Average Time | Worst Time | Space |
| :--- | :--- | :--- | :--- | :--- |
| Linear Search | O(1) | O(n) | O(n) | O(1) |
| Classic Binary Search (Exact match) | O(1) | O(log n) | O(log n) | O(1) |
| Lower Bound (First index >= target) | O(1) | O(log n) | O(log n) | O(1) |
| Upper Bound (First index > target) | O(1) | O(log n) | O(log n) | O(1) |
| Binary Search on Answer Space | O(1) | O(log(Range) * Check) | O(log(Range) * Check) | O(1) |

For Binary Search on Answer, the search space is bounded by the solution range `[low, high]`, and each midpoint check takes O(n) or O(Check) time.

## 3. When to Use
- Array or list is sorted and queries require sub-linear lookup time.
- Finding boundary transition points (e.g., first bad version, peak element, rotation pivot).
- Minimizing the maximum or maximizing the minimum in optimization problems (Binary Search on Answer).
- Searching sorted 2D matrices or range endpoints.

## 4. When NOT to Use
- Data is unsorted and will only be queried once (sorting takes O(n log n), exceeding a single O(n) linear scan).
- Underlying data structure is a linked list (cannot index midpoint in O(1) time).
- Decision predicate is non-monotonic (does not yield `FFFFTTTT` or `TTTTFFFF` pattern).

## 5. Why It Works
Binary search requires monotonicity: if `predicate(mid)` is true, then all values to the right (or left) are also true. Evaluating the midpoint halves the active interval `[left, right]` in every iteration, converging to the boundary in log2(n) steps.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Linear Scan) | Test every potential value from 1 to Range sequentially | O(Range * Check) | O(1) |
| Optimized (Binary Search on Answer) | Halve candidate answer space based on feasibility check | O(log(Range) * Check) | O(1) |

Binary search on answer replaces sequential brute-force testing with exponential interval reduction.

## 7. Data Structures Used Here
- `int[]`: Sorted contiguous array.
- `Arrays.binarySearch()`: Java standard library search utility for sorted arrays.

## 8. Core Template (Java)
```java
// Standard lower bound binary search (First true index)
int binarySearch(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid; // exact match
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1; // not found
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Sorting](../02-sorting/README.md) | [Recursion](../04-recursion/README.md)
