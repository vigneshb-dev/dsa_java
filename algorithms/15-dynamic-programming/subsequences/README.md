# Subsequences DP

> Comparing and optimizing non-contiguous element sequences: LCS, LIS, and Edit Distance.

## 1. Overview
Subsequences DP analyzes relationships between non-contiguous sequences extracted from strings or arrays. Core problems include Longest Common Subsequence (LCS), Longest Increasing Subsequence (LIS), and Edit Distance (Levenshtein Distance). States track index positions across two sequences `dp[i][j]` or the end index of an increasing sequence.

## 2. Time & Space Complexity
| Problem / Variant | Optimal Approach | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Longest Common Subsequence (LCS) | 2D DP Table | O(m * n) | O(m * n) / O(min(m, n)) |
| Edit Distance | 2D DP (insert, delete, replace) | O(m * n) | O(min(m, n)) |
| Longest Increasing Subsequence (LIS) | DP + Patience Binary Search | O(n log n) | O(n) |
| Distinct Subsequences | 2D DP matching string s and t | O(m * n) | O(n) |

LIS can be solved in O(n^2) with simple DP, but optimizing via patience sorting with binary search reduces runtime to O(n log n).

## 3. When to Use
- Comparing two strings for similarity, diff generation, or minimum edit operations.
- Finding the length of the longest monotonically increasing subsequence in an array.
- Matching regex/wildcard patterns containing `*` or `?`.
- DNA sequence alignment in bioinformatics.

## 4. When NOT to Use
- Problem requires strictly contiguous substrings (use Two Pointers, Sliding Window, or KMP).
- Strings are huge (e.g., millions of characters) where quadratic table creation exceeds RAM.
- All possible subsequences must be printed (exponential by nature; requires backtracking).

## 5. Why It Works
For LCS, if `s1[i] == s2[j]`, the match extends the optimal subsequence of previous prefixes: `dp[i][j] = 1 + dp[i-1][j-1]`. If characters differ, the optimal solution must match `s1[0..i-1]` against `s2[0..j]` or vice versa, ensuring comprehensive coverage.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Subsequence Generation) | Generate all 2^m subsequences of s1, check in s2 | O(2^m * n) | O(m) |
| 2D DP Tabulation (LCS) | Match characters incrementally in table | O(m * n) | O(min(m, n)) |

Subsequence DP captures common prefix subproblem results to prevent exponential branch enumeration.

## 7. Data Structures Used Here
- `int[][] dp`: 2D table tracking index pairs `(i, j)`.
- `List<Integer>`: Tails array in LIS binary search patience sorting.

## 8. Core Template (Java)
```java
// Longest Common Subsequence (LCS) 2D DP
int longestCommonSubsequence(String s1, String s2) {
    int m = s1.length(), n = s2.length();
    int[][] dp = new int[m + 1][n + 1];
    for (int i = 1; i <= m; i++) {
        for (int j = 1; j <= n; j++) {
            if (s1.charAt(i - 1) == s2.charAt(j - 1)) {
                dp[i][j] = 1 + dp[i - 1][j - 1];
            } else {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }
    return dp[m][n];
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Dynamic Programming](../README.md) | [Knapsack DP](../knapsack/README.md) | [Partition and Interval DP](../partition-and-interval/README.md)
