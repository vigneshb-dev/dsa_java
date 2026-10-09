# Subsequences Dynamic Programming
> Sequence alignment, common subsegments, and order-preserving subsequence optimization across strings and arrays.

## 1. Overview
Subsequences Dynamic Programming addresses problems comparing or aligning two sequences while preserving relative order (without requiring contiguity). Classic problems include Longest Common Subsequence (LCS), Edit Distance (Levenshtein), Distinct Subsequences, and Longest Increasing Subsequence (LIS).

## 2. Input / Output
- Input: Two strings $s_1$ and $s_2$ (or an array `nums`).
- Output: Length of matching subsequence, minimum edit distance, or count (e.g. `s1 = "abcde"`, `s2 = "ace"` $\to$ LCS length `3`).

## 3. Constraints
- String lengths $N, M \le 2,500 \implies N \times M \le 6.25 \times 10^6$ operations.
- Order is strictly non-decreasing in indices (subsequence property).

## 4. Brute-Force Approach
- Idea: Generate all $2^N$ subsequences of $s_1$ and check if each appears in $s_2$.
- Pseudocode: Recursive branch for match vs mismatch.
- Time: $O(2^{\min(N, M)})$; Space: $O(N + M)$ call stack.

## 5. Optimal Approach
- Idea: Define `dp[i][j]` as the solution for prefixes $s_1[0..i-1]$ and $s_2[0..j-1]$. If characters match (`s1[i-1] == s2[j-1]`), extend diagonal; otherwise take best from top or left.
```java
// Reusable Subsequences DP Template (Longest Common Subsequence, O(M) Space)
public class SubsequencesDPTemplate {
    public static int longestCommonSubsequence(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        int[] prev = new int[m + 1];
        int[] curr = new int[m + 1];

        for (int i = 1; i <= n; i++) {
            char c1 = s1.charAt(i - 1);
            for (int j = 1; j <= m; j++) {
                char c2 = s2.charAt(j - 1);
                if (c1 == c2) {
                    curr[j] = 1 + prev[j - 1]; // Extend diagonal match
                } else {
                    curr[j] = Math.max(prev[j], curr[j - 1]); // Skip char from s1 or s2
                }
            }
            // Swap rolling rows
            int[] temp = prev; prev = curr; curr = temp;
        }

        return prev[m];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the exponential generation of duplicate prefix alignments.
- **Cases skipped:** If characters match, there is no need to consider skipping either character, skipping branches by diagonal greedy alignment.
- **Shortcuts / tricks used:** 1-indexed DP table avoids out-of-bounds checks; rolling row array reduces 2D table to two 1D rows.
- **Time saved:** $O(2^{N+M}) \to O(N \cdot M)$.
- **Space effect:** $O(N \cdot M) \to O(\min(N, M))$ memory.
- **Trade-off:** In-place row space optimization does not retain traceback pointers needed to reconstruct the exact string sequence.

## 6. Core Idea
Comparing prefixes character by character allows the problem to be solved diagonally when characters match, or by choosing the maximum of dropping the last character of $s_1$ versus $s_2$.

## 7. Pattern
- Pattern: Dual-Sequence Prefix Alignment.
- Signals: "Longest common subsequence", "edit distance (insert, delete, replace)", "wildcard matching / regex matching", "interleaving string", "shortest common supersequence".

## 8. Data Structure Used
- Two 1D primitive arrays `int[M + 1]` (or 2D table `int[N + 1][M + 1]`).

## 9. Invariant
At step $(i, j)$, `dp[i][j]` accurately stores the optimal metric for prefix strings $s_1[0..i-1]$ and $s_2[0..j-1]$.

## 10. Dry Run
LCS of `"ac"` and `"abc"`:
| $i \backslash j$ | `""` (0) | `'a'` (1) | `'b'` (2) | `'c'` (3) |
|---|---|---|---|---|
| `""` (0) | 0 | 0 | 0 | 0 |
| `'a'` (1) | 0 | 1 (match) | 1 | 1 |
| `'c'` (2) | 0 | 1 | 1 | 2 (match) |

Final LCS: `dp[2][3] = 2` (`"ac"`).

## 11. Edge Cases
- One or both strings empty: LCS is 0; Edit Distance is the length of the non-empty string.
- Strings identical: returns string length immediately.
- Strings completely disjoint: returns 0.

## 12. Correctness
By induction on prefix lengths: If `s1[i-1] == s2[j-1]`, this character can be safely appended to optimal LCS of prefixes $i-1$ and $j-1$. If not equal, the optimal LCS cannot use both characters at the end, so it must be the max of dropping one or the other.

## 13. Time Complexity
- Best / Average / Worst: $O(N \cdot M)$ evaluating all cell transitions.

## 14. Space Complexity
- Auxiliary Space: $O(\min(N, M))$ using rolling row buffers (or $O(N \cdot M)$ for path reconstruction).

## 15. Can It Be Optimized?
For Longest Increasing Subsequence (LIS) on a single array, Patience Sorting with Binary Search optimizes time from $O(N^2)$ to $O(N \log N)$. Hirschberg's Algorithm reconstructs the LCS string in $O(N \cdot M)$ time and $O(\min(N, M))$ space.

## 16. When Should I Use This Algorithm?
- Finding similarities or differences between two strings or sequences.
- Edit Distance for spell checking and typo correction.
- DNA sequence alignment in computational biology (Needleman-Wunsch).
- Regex and wildcard pattern matching.
- Longest Palindromic Subsequence (LCS between string and its reverse).

## 17. When Should I NOT Use It?
- Contiguous substring matches (use KMP or Sliding Window in $O(N)$ time).
- Finding LIS on a single array (use Binary Search in $O(N \log N)$).
- Strings of length $> 10^5$ (quadratic time times out).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic subsequences DP problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Knapsack DP](../knapsack/README.md) | [Partition & Interval DP](../partition-and-interval/README.md)
