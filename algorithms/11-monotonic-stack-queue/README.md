# Monotonic Stack & Queue
> Maintaining monotonic order within linear collections to discover nearest greater/smaller elements or sliding window extrema in O(n) time.

## 1. Overview
A Monotonic Stack (or Queue) enforces a strictly increasing or decreasing order of elements inside the data structure. When a new element violates the monotonicity, existing dominated elements are popped out. This property allows problems like Next Greater Element, Largest Rectangle in Histogram, and Sliding Window Maximum to be solved in linear $O(n)$ time.

## 2. Input / Output
- Input: An array of numbers (e.g. `nums = [2, 1, 2, 4, 3]`).
- Output: An array mapping each element to its next greater value or sliding window maximum (e.g. `[4, 2, 4, -1, -1]`).

## 3. Constraints
- Scales to $n \le 10^7$ because each element enters and leaves the stack/queue at most once ($O(n)$ total time).
- Works with arbitrary comparable numbers (positive, negative, duplicates).

## 4. Brute-Force Approach
- Idea: For each element at index $i$, run a forward loop scanning $j = i + 1 \dots n - 1$ until finding an element $> nums[i]$.
- Pseudocode:
  ```java
  for (int i = 0; i < n; i++) {
      ans[i] = -1;
      for (int j = i + 1; j < n; j++) {
          if (nums[j] > nums[i]) { ans[i] = nums[j]; break; }
      }
  }
  ```
- Time: $O(n^2)$; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Maintain indices in a stack with decreasing values. When current element `nums[i]` is larger than `nums[stack.peek()]`, current element is the Next Greater Element for the popped index.
```java
// Reusable Monotonic Stack Skeleton (Next Greater Element Template)
import java.util.*;

public class MonotonicStackTemplate {
    public static int[] nextGreaterElements(int[] nums) {
        int n = nums.length;
        int[] result = new int[n];
        Arrays.fill(result, -1);
        ArrayDeque<Integer> stack = new ArrayDeque<>(); // Stores INDICES

        for (int i = 0; i < n; i++) {
            // While stack is non-empty and current element is strictly greater:
            // current element is the Next Greater Element for top of stack
            while (!stack.isEmpty() && nums[i] > nums[stack.peek()]) {
                int prevIndex = stack.pop();
                result[prevIndex] = nums[i];
            }
            stack.push(i); // Push current index onto monotonic stack
        }

        return result;
    }

    // Monotonic Deque Skeleton (Sliding Window Maximum of size k)
    public static int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n - k + 1];
        ArrayDeque<Integer> deque = new ArrayDeque<>(); // Decreasing values

        for (int i = 0; i < n; i++) {
            // 1. Remove expired indices outside window [i - k + 1, i]
            if (!deque.isEmpty() && deque.peekFirst() < i - k + 1) {
                deque.pollFirst();
            }
            // 2. Remove dominated smaller elements from tail
            while (!deque.isEmpty() && nums[deque.peekLast()] <= nums[i]) {
                deque.pollLast();
            }
            // 3. Add current index
            deque.offerLast(i);

            // 4. Record window maximum once first window is formed
            if (i >= k - 1) {
                res[i - k + 1] = nums[deque.peekFirst()];
            }
        }
        return res;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Eliminates redundant forward searches. A smaller element hidden behind a larger element can never serve as the Next Greater Element for any future element, so it is permanently popped.
- **Cases skipped:** Skips inspecting popped elements; elements in the stack are strictly ordered, so comparisons halt immediately once a larger element is met.
- **Shortcuts / tricks used:** Storing indices rather than raw values allows instant calculation of spans, widths, and expiration timestamps.
- **Time saved:** $O(n^2) \to O(n)$; despite nested `while` loop, every index is pushed onto the stack exactly once and popped at most once ($2n$ total operations).
- **Space effect:** Uses $O(n)$ auxiliary stack space to buffer indices.
- **Trade-off:** Incurs $O(n)$ memory for the stack/deque.

## 6. Core Idea
Elements in the stack represent unresolved candidates waiting for their answer. When an incoming element is larger/smaller, it resolves pending candidates in reverse arrival order and eliminates dominated elements that can never be optimal in the future.

## 7. Pattern
- Pattern: Monotonic Decreasing / Increasing Stack or Deque.
- Signals: "Next Greater / Smaller Element", "daily temperatures", "largest rectangle in histogram", "trapping rain water", "sliding window maximum", "sum of subarray minimums".

## 8. Data Structure Used
- `java.util.ArrayDeque<Integer>` used as stack (storing indices) or double-ended queue.

## 9. Invariant
Elements (or referenced values) in the stack maintain strict monotonicity from bottom to top (e.g., monotonic decreasing: `nums[stack.get(0)] > nums[stack.get(1)] > ...`).

## 10. Dry Run
Input `nums = [2, 1, 2, 4]`:
| $i$ | `nums[i]` | Action on Stack | Popped Index & Assigned Value | Stack State (Indices) |
|---|---|---|---|---|
| 0 | 2 | Push 0 | None | `[0]` (val 2) |
| 1 | 1 | $1 \le 2$, push 1 | None | `[0, 1]` (vals 2, 1) |
| 2 | 2 | $2 > 1$, pop 1; $2 \le 2$, push 2 | `res[1] = 2` | `[0, 2]` (vals 2, 2) |
| 3 | 4 | $4 > 2$, pop 2; $4 > 2$, pop 0 | `res[2] = 4`, `res[0] = 4` | `[3]` (val 4) |

End: remaining index 3 defaults to -1.

## 11. Edge Cases
- Circular arrays: run the loop twice from $0$ to $2n - 1$ using modulo index `i % n`.
- Equal elements: carefully determine strict (`>`) vs non-strict (`>=`) monotonicity based on whether duplicates should pop each other.
- All decreasing or all increasing input arrays: stack either empties completely on every step or grows monotonically until completion.

## 12. Correctness
By induction on stack monotonicity: Since stack elements are strictly decreasing, the first element `nums[i]` encountered that exceeds `nums[top]` must be the earliest (and thus nearest) greater element to the right of `top`.

## 13. Time Complexity
- Best / Average / Worst: $O(n)$. Each of the $n$ elements is pushed once and popped at most once.

## 14. Space Complexity
- Auxiliary Space: $O(n)$ in the worst case where input is monotonically sorted.

## 15. Can It Be Optimized?
Time complexity $O(n)$ and space $O(n)$ are already optimal. For primitive arrays, using a raw primitive array `int[] stack` and an integer pointer `top` eliminates `ArrayDeque` boxing overhead.

## 16. When Should I Use This Algorithm?
- Discovering Next Greater Element or Previous Greater Element.
- Finding the Nearest Smaller Element to the left or right of every index.
- Computing maximum area of a histogram or 0-1 binary matrix.
- Finding Sliding Window Maximum in $O(n)$ time.
- Stock span problems or daily temperatures countdown.

## 17. When Should I NOT Use It?
- Problem queries general k-th larger element (use Segment Tree or Binary Search).
- Array values are dynamically updated between queries (use Segment Tree).
- Simple prefix sums or min/max without distance/span dependencies.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic monotonic stack problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [10 - Prefix Sum & Difference Array](../10-prefix-sum-difference-array/README.md) | [12 - Intervals](../12-intervals/README.md)
