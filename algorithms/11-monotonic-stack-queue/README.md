# Monotonic Stack & Queue

> Maintains strictly increasing or decreasing elements to find nearest extremes in linear time.

## 1. Overview
A Monotonic Stack or Monotonic Queue maintains its elements in strictly sorted (monotonic increasing or decreasing) order. When a new element violates the monotonic invariant, existing elements are popped until the invariant is restored. This enables immediate O(1) resolution of 'next greater element', 'next smaller element', and sliding window maximum queries.

## 2. Time & Space Complexity
| Pattern / Problem | Data Structure | Total Time | Space Complexity |
| :--- | :--- | :--- | :--- |
| Next Greater Element | Monotonic Stack | O(n) | O(n) |
| Daily Temperatures | Monotonic Stack | O(n) | O(n) |
| Largest Rectangle in Histogram | Monotonic Stack | O(n) | O(n) |
| Sliding Window Maximum | Monotonic Deque | O(n) | O(k) window size |

Although inner while loops pop elements, every element is pushed and popped at most once, bounding total runtime strictly to O(n).

## 3. When to Use
- Finding the nearest greater or smaller element to the left or right of each array element.
- Problems involving histograms, stock span, or trapped rainwater.
- Sliding window minimum or maximum within a moving window of size k.
- Subarray range minimum/maximum optimizations.

## 4. When NOT to Use
- Finding global extremes rather than nearest local boundaries (use simple `Math.max` tracking).
- Elements require arbitrary removal rather than boundary pop operations.
- Non-linear graph or tree structures without sequential traversal paths.

## 5. Why It Works
If an incoming element `x` is larger than previous elements, those smaller elements can never serve as the 'next greater element' for any subsequent items. Evicting redundant elements maintains a minimal invariant candidate set, resolving queries in amortized constant time.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Nested Loop) | For each element, scan forward until a greater element appears | O(n^2) | O(1) |
| Optimized (Monotonic Stack) | Push indices; pop smaller elements when larger element arrives | O(n) | O(n) |

The monotonic stack eliminates redundant scans by discarding dominated candidates in a single pass.

## 7. Data Structures Used Here
- `ArrayDeque<Integer>`: Efficient stack storing indices of elements.
- `ArrayDeque<Integer>`: Double-ended queue storing candidate indices for sliding window maximum.

## 8. Core Template (Java)
```java
// Next Greater Element (Monotonic Decreasing Stack)
int[] nextGreaterElement(int[] nums) {
    int n = nums.length;
    int[] result = new int[n];
    Deque<Integer> stack = new ArrayDeque<>(); // stores indices

    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && nums[i] > nums[stack.peek()]) {
            int prevIndex = stack.pop();
            result[prevIndex] = nums[i];
        }
        stack.push(i);
    }
    while (!stack.isEmpty()) {
        result[stack.pop()] = -1; // no greater element
    }
    return result;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Prefix Sum & Difference Array](../10-prefix-sum-difference-array/README.md) | [Intervals](../12-intervals/README.md)
