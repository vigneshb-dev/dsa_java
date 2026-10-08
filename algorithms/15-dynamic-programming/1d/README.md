# 1D Dynamic Programming

> Linear state sequences where current state depends on a constant number of previous states.

## 1. Overview
1D Dynamic Programming addresses problems with a single linear state parameter `dp[i]`. Typical transitions look like `dp[i] = f(dp[i-1], dp[i-2], ...)` such as in Climbing Stairs, House Robber, and Decode Ways. Because each step usually references only a fixed number of prior values, auxiliary space can frequently be optimized from O(n) to O(1).

## 2. Time & Space Complexity
| Problem / Variant | State & Transitions | Time | Space (Standard / Optimized) |
| :--- | :--- | :--- | :--- |
| Climbing Stairs | `dp[i] = dp[i-1] + dp[i-2]` | O(n) | O(n) / O(1) |
| House Robber | `dp[i] = max(dp[i-1], dp[i-2] + val)` | O(n) | O(n) / O(1) |
| Decode Ways | Single and two-digit checks | O(n) | O(n) / O(1) |
| Coin Change (Min Coins) | `dp[i] = min(dp[i - c] + 1)` | O(n * coins) | O(n) |

Tracking just 2 or 3 scalar variables (`prev1`, `prev2`) eliminates the need for an entire 1D array.

## 3. When to Use
- Decision choices at step `i` depend only on outcomes of previous steps `i - 1`, `i - 2`, etc.
- Counting total ways to reach step `n`.
- Maximizing rewards or minimizing costs along a linear array without looking ahead.
- Subproblem transitions form a directed path of length n.

## 4. When NOT to Use
- Decision depends on both index and remaining capacity/count (requires 2D state).
- State transitions have cycles (not a DAG).
- A closed-form mathematical formula exists (e.g., Matrix Exponentiation for huge n).

## 5. Why It Works
Because decisions at step `i` cannot alter past results, the principle of optimality holds. Computing states sequentially from `1` to `n` guarantees every prerequisite state is finalized before use.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Recursion | Branch at each step (e.g., rob or skip) | O(2^n) | O(n) stack |
| 1D Tabulation | Iterate from 1 to n updating running variables | O(n) | O(1) space |

1D tabulation caches the best previous decisions, eliminating duplicate branch calculations.

## 7. Data Structures Used Here
- `int[]`: Basic 1D array for tabulation when full history is needed.
- Scalar variables (`prev1`, `prev2`): For O(1) space optimization.

## 8. Core Template (Java)
```java
// Standard 1D DP with space optimization (House Robber pattern)
int rob(int[] nums) {
    int prev2 = 0, prev1 = 0;
    for (int num : nums) {
        int curr = Math.max(prev1, prev2 + num);
        prev2 = prev1;
        prev1 = curr;
    }
    return prev1;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | Start | [2D Grid Dynamic Programming](../2d-grid/README.md)
