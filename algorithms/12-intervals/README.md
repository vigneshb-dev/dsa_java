# Intervals

> Techniques for sorting, merging, and querying overlapping 1D ranges `[start, end]`.

## 1. Overview
Interval problems operate on segments defined by pairs of coordinates `[start, end]`. The predominant strategy involves sorting intervals by their start (or end) times, which aligns potential overlaps chronologically. Common patterns include merging overlapping intervals, inserting new intervals, interval scheduling, and sweep-line algorithms.

## 2. Time & Space Complexity
| Operation / Problem | Core Strategy | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Merge Intervals | Sort by start time, merge overlaps | O(n log n) | O(n) |
| Insert Interval | Scan in-place or rebuild list | O(n) | O(n) |
| Non-overlapping Intervals | Greedy sort by finish time | O(n log n) | O(1) |
| Meeting Rooms II (Min Rooms) | Min-Heap or Sweep-line / 2 Arrays | O(n log n) | O(n) |

Sorting takes O(n log n), while the subsequent merge or greedy sweep runs in single-pass linear O(n) time.

## 3. When to Use
- Problem inputs are pairs of timestamps, time windows, or continuous numeric ranges.
- Consolidating overlapping bookings, calendar slots, or resource allocations.
- Finding concurrent overlaps (e.g., minimum conference rooms required).
- Finding the intersection or union of multiple sorted interval lists.

## 4. When NOT to Use
- Intervals span high-dimensional volumes (use k-d trees or R-trees).
- Coordinates are discrete points without range duration semantics.
- Data is completely static and needs 2D point enclosure queries (use segment trees).

## 5. Why It Works
Sorting by start time ensures that if interval `B` overlaps with interval `A` (where `A.start <= B.start`), then necessarily `B.start <= A.end`. If `B.start > A.end`, no future interval can ever overlap with `A`, allowing `A` to be finalized immediately.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Pairwise Compare) | Compare every interval with every other interval repeatedly | O(n^2) | O(n) |
| Optimized (Sort + Linear Merge) | Sort intervals by start; merge adjacent overlaps sequentially | O(n log n) | O(n) |

Sorting arranges intervals sequentially so that overlaps are confined strictly to adjacent elements.

## 7. Data Structures Used Here
- `int[][]`: Array representation of intervals `[start, end]`.
- `PriorityQueue<Integer>`: Min-heap storing active end times in Meeting Rooms problems.
- `List<int[]>`: Dynamic list for collecting merged intervals.

## 8. Core Template (Java)
```java
// Standard Merge Intervals Skeleton
int[][] merge(int[][] intervals) {
    if (intervals.length <= 1) return intervals;
    // 1. Sort by start time
    Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));
    List<int[]> merged = new ArrayList<>();
    int[] curr = intervals[0];
    merged.add(curr);

    for (int[] next : intervals) {
        if (next[0] <= curr[1]) { // Overlap detected
            curr[1] = Math.max(curr[1], next[1]);
        } else {
            curr = next;
            merged.add(curr);
        }
    }
    return merged.toArray(new int[merged.size()][]);
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Monotonic Stack & Queue](../11-monotonic-stack-queue/README.md) | [Cyclic Sort](../13-cyclic-sort/README.md)
