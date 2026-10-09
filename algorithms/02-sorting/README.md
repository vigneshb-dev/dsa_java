# Sorting
> Systematic reordering of elements into non-decreasing or non-increasing sequence.

## 1. Overview
Sorting arranges a collection of elements into a monotonically ordered sequence based on a defined comparison key. It serves as a foundational preprocessing primitive that enables efficient binary search, two-pointer scanning, interval merging, and duplicate detection.

## 2. Input / Output
- Input: An unsorted array or list of comparable elements (e.g. `int[] arr = {5, 2, 8, 1, 9}`).
- Output: A permuted array containing identical elements in sorted order (e.g. `{1, 2, 5, 8, 9}`).

## 3. Constraints
- General comparison sorting applies to $n \le 10^6$ for $O(n \log n)$ algorithms.
- Assumes elements are mutually comparable with a total ordering (`a <= b` and `b <= c` $\implies$ `a <= c`).
- Comparison sort lower bound is mathematically proven to be $\Omega(n \log n)$. Non-comparison integer sorting (Counting/Radix) achieves $O(n + k)$ when range $k$ is bounded.

## 4. Brute-Force Approach
- Idea: Compare all adjacent pairs or scan repeatedly for minimum element (Bubble Sort / Selection Sort).
- Pseudocode:
  ```java
  for (int i = 0; i < n; i++) {
      for (int j = i + 1; j < n; j++) {
          if (arr[j] < arr[i]) { swap(arr, i, j); }
      }
  }
  ```
- Time: $O(n^2)$ pairwise comparisons; Space: $O(1)$ in-place.

## 5. Optimal Approach
- Idea: Divide and Conquer (Merge Sort / Quick Sort) or complete binary tree selection (Heap Sort) dividing the problem into logarithmic stages.
```java
// Generic Divide-and-Conquer Sorting Skeleton (Merge Sort Template)
public class SortingTemplate {
    public static void sort(int[] arr) {
        if (arr == null || arr.length <= 1) return;
        int[] temp = new int[arr.length];
        mergeSort(arr, 0, arr.length - 1, temp);
    }

    private static void mergeSort(int[] arr, int left, int right, int[] temp) {
        if (left >= right) return;
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid, temp);
        mergeSort(arr, mid + 1, right, temp);
        merge(arr, left, mid, right, temp);
    }

    private static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        System.arraycopy(arr, left, temp, left, right - left + 1);
        int i = left, j = mid + 1, k = left;
        while (i <= mid && j <= right) {
            if (temp[i] <= temp[j]) arr[k++] = temp[i++];
            else arr[k++] = temp[j++];
        }
        while (i <= mid) arr[k++] = temp[i++];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes redundant full array rescanning for every single element position.
- **Cases skipped:** Merging sorted sub-lists exploits existing sortedness; items in disjoint halves are never compared against each other individually.
- **Shortcuts / tricks used:** Halving problem size recursively achieves tree depth $\log_2 n$; each level processes $n$ total elements.
- **Time saved:** $O(n^2) \to O(n \log n)$; reducing total comparisons from $\approx n^2/2$ to $n \log_2 n$.
- **Space effect:** Uses $O(n)$ auxiliary array buffer in merge sort (or $O(\log n)$ stack space in quicksort) to store partition state.
- **Trade-off:** Pays memory or stack space and recursion overhead to achieve logarithmic comparison depth.

## 6. Core Idea
Divide the problem space hierarchically into subproblems of size $n/2$, solve subproblems independently, and recombine them in linear time. The resulting recursion tree has height $\log_2 n$ with linear work per level.

## 7. Pattern
- Pattern: Divide and Conquer / Comparison Sorting.
- Signals: "Sort array", "find median", "merge intervals", "rank order", "find k-th element".

## 8. Data Structure Used
- Temporary buffer array `int[] temp` for merge storage.
- Stack frames for recursion (depth bounded by $\log n$).

## 9. Invariant
At level completion, sub-arrays `arr[left..mid]` and `arr[mid+1..right]` are sorted, and the merged sub-array `arr[left..right]` contains all their elements in non-decreasing order.

## 10. Dry Run
Sorting array `[4, 2, 7, 1]`:
| Step | Left Subarray | Right Subarray | Action | Resulting Segment |
|---|---|---|---|---|
| 1 | `[4]` | `[2]` | Merge `[4]` & `[2]` | `[2, 4]` |
| 2 | `[7]` | `[1]` | Merge `[7]` & `[1]` | `[1, 7]` |
| 3 | `[2, 4]` | `[1, 7]` | Two-pointer merge | `[1, 2, 4, 7]` |

## 11. Edge Cases
- Empty array (`length == 0`) or single-element array (`length == 1`): return immediately.
- Array with identical duplicate values: stable comparator (`<=`) preserves relative order.
- Already sorted or reverse sorted input: randomized pivot or 3-way partition avoids $O(n^2)$ quicksort degradation.

## 12. Correctness
By structural induction: Base cases of size 1 are sorted. If sub-arrays of size $< n$ are sorted, the two-pointer merge combines them by taking the smaller available element, ensuring every merged prefix is monotonic.

## 13. Time Complexity
- Best: $O(n \log n)$ (or $O(n)$ for adaptive Timsort on presorted runs).
- Average: $O(n \log n)$ via balanced divide-and-conquer recursion tree.
- Worst: $O(n \log n)$ (MergeSort/HeapSort) or $O(n^2)$ (unbalanced QuickSort).

## 14. Space Complexity
- Auxiliary Space: $O(n)$ for MergeSort buffer; $O(1)$ for HeapSort; $O(\log n)$ call stack for QuickSort.

## 15. Can It Be Optimized?
Comparison sorting cannot beat the theoretical $\Omega(n \log n)$ lower bound (decision tree model). Non-comparison sorting (Counting Sort, Radix Sort) achieves $O(n + k)$ when keys are bounded integers.

## 16. When Should I Use This Algorithm?
- Preprocessing step before binary search or two-pointer passes.
- Finding duplicates or grouping identical items.
- Interval scheduling and interval overlap detection.
- Finding minimum or maximum differences between adjacent numbers.
- Generating rank orders and leaderboard standings.

## 17. When Should I NOT Use It?
- Only finding the single minimum or maximum element (use a single $O(n)$ pass).
- Only finding the k-th smallest element where $k \ll n$ (use Quickselect in $O(n)$ or a heap in $O(n \log k)$).
- Keys are small bounded integers within range $k \ll n \log n$ (use Counting Sort).

---

## Comparison Table
| Algorithm | Time (Best / Avg / Worst) | Space | Stable | Best Use |
|---|---|---|---|---|
| Merge Sort | $O(n \log n) / O(n \log n) / O(n \log n)$ | $O(n)$ | Yes | Linked lists, guaranteed worst-case, stability needed |
| Quick Sort | $O(n \log n) / O(n \log n) / O(n^2)$ | $O(\log n)$ | No | In-place primitive array sorting, cache efficiency |
| Heap Sort | $O(n \log n) / O(n \log n) / O(n \log n)$ | $O(1)$ | No | Systems requiring guaranteed $O(n \log n)$ and $O(1)$ space |
| Insertion Sort | $O(n) / O(n^2) / O(n^2)$ | $O(1)$ | Yes | Small arrays ($n < 32$), nearly sorted data |
| Counting Sort | $O(n + k) / O(n + k) / O(n + k)$ | $O(k)$ | Yes | Small integer range $k = \max - \min \le 10^6$ |
| Radix Sort | $O(d(n + k)) / O(d(n + k)) / O(d(n + k))$ | $O(n + k)$ | Yes | Fixed-width integers or strings ($d$ digits) |

---

### Algorithm: Merge Sort
- **Input / Output:** Array of $n$ elements $\to$ Sorted array. E.g., `[3, 1, 2]` $\to$ `[1, 2, 3]`.
- **Constraints:** $n \le 10^6$, arbitrary comparable objects.
- **Brute Force:** Compare and swap all pairs ($O(n^2)$ time, $O(1)$ space).
- **Optimal Approach:** Recursively split array into halves and merge in sorted order.
- **How It Reduces Time/Space:** Eliminates cross-comparisons; reduces time from $O(n^2)$ to $O(n \log n)$ at the cost of $O(n)$ temporary buffer.
- **Core Idea:** Merging two pre-sorted lists takes linear time.
- **Pattern:** Divide and conquer.
- **Data Structure Used:** Auxiliary array `temp[]` for merging.
- **Invariant:** After each merge, the active subarray segment is sorted.
- **Dry Run:** `[2, 1]` split to `[2]` and `[1]`, merged to `[1, 2]`.
- **Edge Cases:** Single elements, duplicate keys (stability preserves ordering).
- **Correctness:** Proved by structural induction on subarray size.
- **Time Complexity:** Best/Avg/Worst: $O(n \log n)$, strictly bounded recursion tree.
- **Space Complexity:** $O(n)$ auxiliary array buffer + $O(\log n)$ call stack.
- **Can It Be Optimized:** Already optimal for comparison-based stable sorting.
- **When to Use:** Stability required; sorting linked lists; external merge sort for disk files.
- **When NOT to Use:** Strict $O(1)$ memory constraint (use HeapSort) or cache-sensitive primitive sorting (use QuickSort).

### Algorithm: Quick Sort
- **Input / Output:** Array of $n$ items $\to$ In-place sorted array. E.g., `[4, 1, 3]` $\to$ `[1, 3, 4]`.
- **Constraints:** $n \le 10^6$, in-place primitive array sorting.
- **Brute Force:** Bubble sort scanning all adjacent elements ($O(n^2)$ time, $O(1)$ space).
- **Optimal Approach:** Choose pivot, partition elements into $< pivot$ and $> pivot$, recurse on partitions.
- **How It Reduces Time/Space:** In-place partitioning skips comparing elements in left partition against right partition.
- **Core Idea:** Pivot is placed into its final sorted position in each pass.
- **Pattern:** Divide and conquer with in-place two-way or three-way partitioning.
- **Data Structure Used:** Input array in-place, recursion call stack.
- **Invariant:** Elements left of pivot $\le pivot$, elements right $\ge pivot$.
- **Dry Run:** `[3, 1, 4]`, pivot 3 $\implies$ partitioned to `[1]`, `3`, `[4]`.
- **Edge Cases:** Duplicate keys (use 3-way Dutch National Flag partition), presorted input (use random pivot).
- **Correctness:** Follows from partition correctness and induction on sub-arrays.
- **Time Complexity:** Best/Avg: $O(n \log n)$; Worst: $O(n^2)$ on unbalanced pivots.
- **Space Complexity:** $O(\log n)$ recursion stack space with tail-call optimization.
- **Can It Be Optimized:** Dual-pivot quicksort (Java's default for primitives) reduces memory traffic.
- **When to Use:** Fast general-purpose in-place sorting for primitives with high cache locality.
- **When NOT to Use:** Stable ordering required (use MergeSort) or guaranteed worst-case required without randomized pivots (use HeapSort).

### Algorithm: Heap Sort
- **Input / Output:** Unsorted array $\to$ In-place sorted array. E.g., `[9, 2, 5]` $\to$ `[2, 5, 9]`.
- **Constraints:** $n \le 10^6$, strict $O(1)$ memory environments.
- **Brute Force:** Repeatedly scan entire array for maximum and place at end ($O(n^2)$ Selection Sort).
- **Optimal Approach:** Transform array into Max-Heap in $O(n)$; repeatedly extract max and sift down in $O(\log n)$.
- **How It Reduces Time/Space:** Heap property eliminates $O(n)$ scan per element; achieves $O(n \log n)$ in $O(1)$ auxiliary space.
- **Core Idea:** Complete binary tree packed into array allows extraction of maximum in logarithmic time.
- **Pattern:** Selection sort accelerated by heap priority queue.
- **Data Structure Used:** Array-backed Max-Heap.
- **Invariant:** Array suffix `arr[i..n-1]` is sorted and contains the largest elements.
- **Dry Run:** Heapify `[3, 1, 4]` $\to$ `[4, 1, 3]`; swap root 4 to end $\to$ `[3, 1, 4]`.
- **Edge Cases:** Already heapified input, all duplicates.
- **Correctness:** Every extraction removes global maximum among remaining elements.
- **Time Complexity:** Best/Avg/Worst: $O(n \log n)$ guaranteed in all cases.
- **Space Complexity:** $O(1)$ auxiliary memory (strictly in-place).
- **Can It Be Optimized:** Optimal comparison sort; slightly slower cache performance than QuickSort due to non-local heap pointer jumps.
- **When to Use:** Embedded systems requiring guaranteed $O(n \log n)$ with zero auxiliary heap/stack allocation.
- **When NOT to Use:** Stable sorting required (use MergeSort) or maximum cache throughput needed (use QuickSort).

### Algorithm: Counting Sort
- **Input / Output:** Integer array with range $[0, k]$ $\to$ Sorted array. E.g., `[4, 2, 2, 8]` $\to$ `[2, 2, 4, 8]`.
- **Constraints:** Integer values with small range $k = \max - \min \le 10^6$.
- **Brute Force:** Comparison sort in $O(n \log n)$.
- **Optimal Approach:** Count frequencies in `count[k]`, prefix sum frequencies, place elements into output.
- **How It Reduces Time/Space:** Non-comparison indexing bypasses $\Omega(n \log n)$ lower bound; reduces time to $O(n + k)$.
- **Core Idea:** Position of element $x$ is directly determined by the count of elements smaller than $x$.
- **Pattern:** Frequency hashing / bucket counting.
- **Data Structure Used:** Frequency count array `int[k]` and output array `int[n]`.
- **Invariant:** Elements placed into output array preserve stable relative input ordering.
- **Dry Run:** Input `[2, 0, 2]`; count: `{0:1, 1:0, 2:2}`; output placed in order.
- **Edge Cases:** Negative numbers (offset by minimum), empty input.
- **Correctness:** Direct bijective mapping from integer value to calculated rank.
- **Time Complexity:** Best/Avg/Worst: $O(n + k)$ linear time.
- **Space Complexity:** $O(n + k)$ auxiliary space for counts and output buffer.
- **Can It Be Optimized:** In-place counting sort saves $O(n)$ space at the expense of stability.
- **When to Use:** Small integer ranges ($k \approx O(n)$); subroutine in Radix Sort.
- **When NOT to Use:** Arbitrary objects, floating point numbers, or massive range $k \gg n \log n$ (use QuickSort/MergeSort).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic sorting problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [01 - Complexity Analysis](../01-complexity-analysis/README.md) | [03 - Searching](../03-searching/README.md)
