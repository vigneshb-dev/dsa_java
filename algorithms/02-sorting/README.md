# Sorting

> Reordering elements into monotonic order to enable efficient searching and greedy processing.

## 1. Overview
Sorting arranges elements of a collection into a designated order (ascending or descending). Efficient comparison-based sorts like Merge Sort and Quick Sort achieve O(n log n) average time, which is mathematically optimal for comparison models. Java's standard library uses Dual-Pivot Quicksort for primitive arrays (`Arrays.sort(int[])`) and Timsort for object arrays (`Arrays.sort(T[])`).

## 2. Time & Space Complexity
| Algorithm | Best Time | Average Time | Worst Time | Space | Stable? |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Bubble Sort | O(n) | O(n^2) | O(n^2) | O(1) | Yes |
| Insertion Sort | O(n) | O(n^2) | O(n^2) | O(1) | Yes |
| Selection Sort | O(n^2) | O(n^2) | O(n^2) | O(1) | No |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n^2) | O(log n) stack | No |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes |

Timsort (used in Java for objects) runs in O(n) best-case time on already sorted or partially sorted arrays by identifying natural monotonic runs.

## 3. When to Use
- Enabling binary search or two-pointer techniques on an initially unordered collection.
- Interval problems requiring ordering by start or finish times.
- Greedy problems requiring processing largest or smallest elements first.
- Detecting duplicates or finding the median/order statistics.

## 4. When NOT to Use
- Data is already sorted or nearly sorted (insertion sort or direct linear scan suffices).
- Only the Top-K elements are required (a size-k Heap is faster at O(n log k) than O(n log n) full sort).
- Order of arrival must strictly be preserved without any comparator rearrangements.

## 5. Why It Works
Comparison trees prove that any comparison sort requires at least log2(n!) = Omega(n log n) comparisons in the worst case. Merge Sort achieves this by recursively halving the array and merging sorted halves in linear O(n) time.

## 6. Brute Force vs Optimized
| Approach Category | Algorithms | Average Time | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Simple Sorts | Bubble Sort, Selection Sort, Insertion Sort | O(n^2) | O(1) |
| Efficient Sorts | Merge Sort, Quick Sort, Heap Sort, Timsort | O(n log n) | O(1) to O(n) |

Efficient sorts trade a divide-and-conquer recursion stack and auxiliary merge buffer to cut quadratic O(n^2) time down to O(n log n).

## 7. Data Structures Used Here
- `Arrays.sort()`: Dual-Pivot Quicksort for primitives, Timsort for objects.
- `Collections.sort()`: Delegates to `List.sort()` (Timsort).
- `PriorityQueue`: Used for Heap Sort.

## 8. Core Template (Java)
```java
// Standard Quick Sort partition skeleton
void quickSort(int[] arr, int low, int high) {
    if (low < high) {
        int p = partition(arr, low, high);
        quickSort(arr, low, p - 1);
        quickSort(arr, p + 1, high);
    }
}

int partition(int[] arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            int tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
        }
    }
    int tmp = arr[i + 1]; arr[i + 1] = arr[high]; arr[high] = tmp;
    return i + 1;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Complexity Analysis](../01-complexity-analysis/README.md) | [Searching](../03-searching/README.md)
