# Intervals
> Sorting and scanning 1D continuous segments to resolve overlaps, merges, and scheduling conflicts.

## 1. Overview
Interval algorithms process 1D segments characterized by start and end timestamps $[start, end]$ along a continuous timeline. By sorting intervals by their start or end boundaries, complex combinatorial overlap relationships reduce to a single linear sweep that merges overlapping ranges or schedules non-conflicting activities.

## 2. Input / Output
- Input: An array of intervals $[start_i, end_i]$ (e.g. `[[1, 3], [2, 6], [8, 10], [15, 18]]`).
- Output: A merged, non-overlapping array of intervals, or minimum resource count (e.g. `[[1, 6], [8, 10], [15, 18]]`).

## 3. Constraints
- Scales to $n \le 10^6$ intervals bounded by $O(n \log n)$ sorting.
- Boundary points can be large integers up to $10^9$ (sorting handles arbitrary coordinate values without dense arrays).

## 4. Brute-Force Approach
- Idea: Compare every interval against every other interval iteratively. If two overlap, merge them and restart the scan until no more merges occur.
- Pseudocode:
  ```java
  boolean merged = true;
  while (merged) {
      merged = false;
      for (int i = 0; i < list.size(); i++) {
          for (int j = i + 1; j < list.size(); j++) {
              if (overlaps(list.get(i), list.get(j))) {
                  list.set(i, merge(list.get(i), list.get(j)));
                  list.remove(j);
                  merged = true; break;
              }
          }
      }
  }
  ```
- Time: $O(n^2)$ worst case; Space: $O(n)$.

## 5. Optimal Approach
- Idea: Sort intervals primarily by start time ($start_i$). Iterate sequentially: if the current interval's start is $\le$ previous interval's end, merge them by extending the end boundary to $\max(prevEnd, currEnd)$; otherwise, push the completed interval.
```java
// Reusable Merge Intervals Skeleton
import java.util.*;

public class IntervalsTemplate {
    public static int[][] mergeIntervals(int[][] intervals) {
        if (intervals == null || intervals.length <= 1) return intervals;

        // 1. Sort intervals by start time ascending
        Arrays.sort(intervals, (a, b) -> Integer.compare(a[0], b[0]));

        List<int[]> merged = new ArrayList<>();
        int[] current = intervals[0];
        merged.add(current);

        // 2. Linear sweep through sorted intervals
        for (int i = 1; i < intervals.length; i++) {
            int[] next = intervals[i];

            if (next[0] <= current[1]) {
                // Overlap detected: extend end boundary
                current[1] = Math.max(current[1], next[1]);
            } else {
                // Disjoint interval: start a new interval
                current = next;
                merged.add(current);
            }
        }

        return merged.toArray(new int[merged.size()][]);
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the need to compare an interval against all other $n - 1$ intervals.
- **Cases skipped:** Because intervals are sorted by start time, once an interval begins after the current interval has ended ($next[0] > current[1]$), it is mathematically guaranteed that ALL subsequent intervals will also begin after `current[1]`.
- **Shortcuts / tricks used:** Sorting by start time organizes intervals chronologically, reducing multi-dimensional pairwise checks to adjacent comparisons.
- **Time saved:** $O(n^2) \to O(n \log n)$; the bottleneck becomes the $O(n \log n)$ sort followed by a single $O(n)$ linear sweep.
- **Space effect:** Allocates $O(n)$ space for the merged result list.
- **Trade-off:** Requires upfront sorting of the interval collection.

## 6. Core Idea
Chronological sorting ensures that overlapping intervals are grouped adjacently. Maintaining a running active interval allows continuous extension until a gap appears, at which point the current interval is finalized.

## 7. Pattern
- Pattern: Line Sweep / Chronological Interval Sorting.
- Signals: "Merge overlapping intervals", "insert new interval", "meeting rooms (min conference rooms)", "non-overlapping intervals (min deletions to make non-overlapping)", "interval intersection".

## 8. Data Structure Used
- Priority Queue (Min-Heap) for concurrent active intervals (e.g. tracking room end times in Meeting Rooms II).
- Dynamic List `List<int[]>` for output collection.

## 9. Invariant
At step $i$, all intervals prior to $i$ have been condensed into a strictly disjoint, sorted list of non-overlapping intervals, and `current` holds the active interval spanning up to the latest known end.

## 10. Dry Run
Merging `[[1, 3], [2, 6], [8, 10]]`:
| Step | Active Interval | Next Interval | Overlap Check | Action | Merged Output |
|---|---|---|---|---|---|
| Init | `[1, 3]` | - | - | Add to output | `[[1, 3]]` |
| 1 | `[1, 3]` | `[2, 6]` | $2 \le 3$ (True) | Update end = $\max(3, 6) = 6$ | `[[1, 6]]` |
| 2 | `[1, 6]` | `[8, 10]` | $8 \le 6$ (False) | Finalize `[1, 6]`, start `[8, 10]` | `[[1, 6], [8, 10]]` |

## 11. Edge Cases
- Intervals touching at a single point (e.g. `[1, 2]` and `[2, 3]`): whether they merge depends on if intervals are closed (`start <= end`) or open (`start < end`).
- Fully nested intervals (e.g. `[1, 10]` and `[2, 5]`): correctly resolved by taking $\max(currEnd, nextEnd)$.
- Empty input array or single interval: early return.

## 12. Correctness
By induction: Sorting ensures $start_0 \le start_1 \le \dots \le start_{n-1}$. For any interval $i$, if $start_i \le end_{active}$, they overlap by definition and must form a single connected component $[start_{active}, \max(end_{active}, end_i)]$. If $start_i > end_{active}$, no future interval $j > i$ can overlap with $active$ since $start_j \ge start_i > end_{active}$.

## 13. Time Complexity
- Best / Average / Worst: $O(n \log n)$, strictly dominated by sorting the $n$ intervals. The subsequent merge loop is $O(n)$.

## 14. Space Complexity
- Auxiliary Space: $O(n)$ for sorting call stack and output array.

## 15. Can It Be Optimized?
If the intervals are already provided in sorted order, the algorithm runs in optimal $O(n)$ time and $O(1)$ extra space.

## 16. When Should I Use This Algorithm?
- Merging overlapping appointment intervals or IP address ranges.
- Finding the minimum number of meeting rooms or resources required (sweep-line event sorting).
- Inserting a new interval into a pre-existing list of sorted disjoint intervals.
- Activity selection / maximum number of non-overlapping intervals (sort by end time).
- Computing interval intersection sets.

## 17. When Should I NOT Use It?
- Multi-dimensional geometric bounding boxes (2D rectangles; use R-Trees or KD-Trees).
- Querying dynamic range sums or updates (use Segment Trees or Fenwick Trees).
- Disjoint set connectivity where order is irrelevant (use Union-Find).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic intervals problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [11 - Monotonic Stack & Queue](../11-monotonic-stack-queue/README.md) | [13 - Cyclic Sort](../13-cyclic-sort/README.md)
