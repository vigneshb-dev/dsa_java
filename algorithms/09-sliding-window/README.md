# Sliding Window

> Maintains an active window over sequential elements to track contiguous range properties.

## 1. Overview
The Sliding Window pattern maintains a contiguous subsegment (window) bounded by two indices `[left, right]` over an array or string. As the right pointer expands the window to include new elements, the left pointer contracts the window when constraints are violated. This transforms quadratic O(n^2) subarray or substring searches into linear O(n) time by reusing calculations from overlapping windows.

## 2. Time & Space Complexity
| Variant | Window Behavior | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Fixed Size Window | Right advances, left advances concurrently at distance k | O(n) | O(1) or O(Alphabet) |
| Dynamic Window (Max Substring) | Right expands; left contracts while condition invalid | O(n) | O(Alphabet) |
| Dynamic Window (Min Substring) | Right expands until valid; left contracts while still valid | O(n) | O(Alphabet) |
| Monotonic Sliding Window | Track running min/max via ArrayDeque | O(n) | O(k) |

Although the nested while loop advances `left`, both `left` and `right` traverse the array at most once, bounding total pointer steps to 2*n = O(n).

## 3. When to Use
- Problem asks for longest, shortest, or target contiguous subarray or substring.
- Subarray criteria involves running sums, character frequencies, or distinct element counts.
- Fixed length subarray calculations (e.g., maximum sum subarray of size k).
- Strings with anagram substring matching or character replacement limits.

## 4. When NOT to Use
- Problem requires non-contiguous subsequences or arbitrary subsets (use DP or recursion).
- Array contains negative numbers and target is exact sum (prefix sums + hash map must be used because monotonicity is lost).
- Dataset does not maintain sequential order.

## 5. Why It Works
Window updates maintain running state incrementally: `add(right)` when expanding, `remove(left)` when contracting. Because elements enter and exit the window exactly once, all valid windows are evaluated in amortized O(1) time per element.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (All Subarrays) | Check every pair `(i, j)` and re-evaluate validity | O(n^2) or O(n^3) | O(1) |
| Optimized (Sliding Window) | Expand right pointer and shrink left pointer dynamically | O(n) | O(Alphabet) |

Sliding window avoids re-scanning overlapping elements by adjusting boundary counters incrementally.

## 7. Data Structures Used Here
- `int[] freq`: Character frequency counter array for ASCII strings.
- `HashMap<Character, Integer>`: Dynamic frequency counter for arbitrary character alphabets.
- `ArrayDeque<Integer>`: Stores indices for monotonic sliding window min/max.

## 8. Core Template (Java)
```java
// Standard Dynamic Sliding Window (Longest Valid Substring)
int longestWindow(String s) {
    int[] map = new int[128];
    int left = 0, maxLength = 0;
    for (int right = 0; right < s.length(); right++) {
        char rChar = s.charAt(right);
        map[rChar]++;
        // Contract window while constraint is violated
        while (map[rChar] > 1) {
            char lChar = s.charAt(left);
            map[lChar]--;
            left++;
        }
        maxLength = Math.max(maxLength, right - left + 1);
    }
    return maxLength;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Fast & Slow Pointers](../08-fast-slow-pointers/README.md) | [Prefix Sum & Difference Array](../10-prefix-sum-difference-array/README.md)
