# Arrays

> Fixed-size contiguous memory blocks providing O(1) random index access.

## 1. Overview
An array is a linear data structure that stores elements of identical type in contiguous memory locations. Because each element occupies a uniform number of bytes, any index can be accessed in O(1) time using base address arithmetic. In Java, primitive arrays have a fixed length upon allocation, while dynamic arrays like ArrayList resize dynamically.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Access by Index | O(1) / O(1) / O(1) | O(1) |
| Update by Index | O(1) / O(1) / O(1) | O(1) |
| Search (unsorted) | O(1) / O(n) / O(n) | O(1) |
| Search (sorted, binary) | O(1) / O(log n) / O(log n) | O(1) |
| Insert at End (ArrayList) | O(1) / O(1) amortized / O(n) worst | O(1) |
| Insert / Delete at Middle | O(1) / O(n) / O(n) | O(1) |

Insertion at the end of an ArrayList is amortized O(1) because geometric array resizing (1.5x growth in Java) amortizes reallocation cost.

## 3. When to Use
- Random access by index in O(1) time is required repeatedly.
- The total number of elements is known beforehand or has a small fixed upper bound.
- Cache locality and contiguous memory storage are critical for high-throughput iteration.
- Passing primitive elements without boxing overhead (e.g., `int[]` instead of `Integer[]`).
- Working as the backing buffer for other data structures like heaps, hash tables, and ring buffers.

## 4. When NOT to Use
- Frequent arbitrary insertions or deletions are needed at the beginning or middle (prefer `LinkedList` or `ArrayDeque`).
- Dataset size varies widely and reallocations cause latency spikes or memory waste.
- Fast key-based lookups or non-integer indices are required (prefer `HashMap`).

## 5. Why It Works
Array random access works because the memory address of element `i` is computed via `Address(i) = BaseAddress + i * ElementSize`. Because this arithmetic is performed in a single processor instruction cycle, index access is constant time O(1). Contiguous memory layout also maximizes CPU cache line utilization during sequential traversal.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Linear Scan) | Check every pair or element iteratively | O(n^2) | O(1) |
| Optimized (Sorted + Two Pointers) | Sort array and scan inward from both ends | O(n log n) | O(1) |

The optimized approach trades initial sorting time to eliminate an entire nested loop from O(n^2) to O(n log n).

## 7. Data Structures Used Here
- `int[]` / `Object[]`: Primitive or reference array in Java; contiguous chunk allocated directly on the heap.
- `ArrayList<E>`: Resizable array implementation backed by an `Object[]` array with 50% growth rate (`newCapacity = oldCapacity + (oldCapacity >> 1)`).

## 8. Core Template (Java)
```java
// Dynamic array initialization and standard traversal
int[] nums = new int[]{10, 20, 30, 40, 50};
for (int i = 0; i < nums.length; i++) {
    int val = nums[i];
    // Process nums[i]
}

// Two-pointer array transformation in-place
int left = 0, right = nums.length - 1;
while (left < right) {
    int temp = nums[left];
    nums[left] = nums[right];
    nums[right] = temp;
    left++;
    right--;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | Start | [Strings](../02-strings/README.md)
