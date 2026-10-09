# String Algorithms
> Exact pattern matching, rolling hashes, and linear-time palindromic transformations over character sequences.

## 1. Overview
String Algorithms process sequences of characters to solve pattern searching, substring indexing, and symmetry analysis in linear time. Rather than relying on quadratic brute-force comparisons, advanced algorithms like Knuth-Morris-Pratt (KMP), Rabin-Karp, Z-Algorithm, and Manacher's algorithm preprocess strings to avoid backtracking across the text.

## 2. Input / Output
- Input: Text string $T$ and pattern string $P$ (e.g. $T = \text{"ababcababa"}$, $P = \text{"ababa"}$).
- Output: Starting index of first or all occurrences of $P$ in $T$ (e.g. index `5`).

## 3. Constraints
- Text length $N \le 10^7$, Pattern length $M \le 10^6$.
- Operates in linear time $O(N + M)$ with alphabet size $\Sigma$.

## 4. Brute-Force Approach
- Idea: Slide the pattern across the text and check character-by-character from each starting index.
- Pseudocode:
  ```java
  for (int i = 0; i <= n - m; i++) {
      int j = 0;
      while (j < m && text.charAt(i + j) == pat.charAt(j)) j++;
      if (j == m) return i;
  }
  ```
- Time: $O(N \cdot M)$ worst case (e.g. $T = \text{"aaaaa"} , P = \text{"aaaab"}$); Space: $O(1)$.

## 5. Optimal Approach
- Idea: Precompute the Longest Proper Prefix which is also a Suffix (LPS array / $\pi$ table) in $O(M)$ time. When a mismatch occurs at `pat[j]`, shift the pattern to `j = lps[j - 1]` without resetting text pointer $i$.
```java
// Reusable Knuth-Morris-Pratt (KMP) String Matching Template
public class StringAlgorithmsTemplate {
    public static int kmpSearch(String text, String pattern) {
        int n = text.length(), m = pattern.length();
        if (m == 0) return 0;

        // 1. Precompute LPS (Longest Prefix Suffix) array
        int[] lps = computeLPS(pattern);

        int i = 0, j = 0; // i for text, j for pattern
        while (i < n) {
            if (text.charAt(i) == pattern.charAt(j)) {
                i++;
                j++;
                if (j == m) {
                    return i - j; // Match found at starting index i - j
                }
            } else {
                if (j != 0) {
                    j = lps[j - 1]; // Fall back in pattern without resetting i
                } else {
                    i++; // No prefix match; advance in text
                }
            }
        }
        return -1;
    }

    private static int[] computeLPS(String pat) {
        int m = pat.length();
        int[] lps = new int[m];
        int len = 0, i = 1;

        while (i < m) {
            if (pat.charAt(i) == pat.charAt(len)) {
                len++;
                lps[i++] = len;
            } else {
                if (len != 0) len = lps[len - 1];
                else lps[i++] = 0;
            }
        }
        return lps;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Prevents rolling the text index $i$ backward upon a mismatch; text pointer $i$ moves strictly monotonically forward.
- **Cases skipped:** If pattern prefix has no overlap with its suffix, the pattern leaps forward past all mismatched characters in $O(1)$.
- **Shortcuts / tricks used:** LPS array encodes self-similarity of pattern, using past match knowledge to determine the longest reusable prefix.
- **Time saved:** $O(N \cdot M) \to O(N + M)$ strictly guaranteed.
- **Space effect:** Allocates $O(M)$ auxiliary memory for the LPS table.
- **Trade-off:** Incurs $O(M)$ precomputation before scanning the text.

## 6. Core Idea
Never move backward in the text. By precomputing how the pattern matches with itself, a mismatch indicates exactly what prefix of the pattern is already valid at the current position.

## 7. Pattern
- Pattern: Deterministic Finite Automaton (DFA) Simulation / Rolling Hash.
- Signals: "Find substring occurrence", "longest happy prefix", "repeated substring pattern", "longest palindromic substring in O(N)", "multiple pattern matching".

## 8. Data Structure Used
- Primitive integer array `int[M]` for LPS or Z-array.
- Long integer hash registers for Rabin-Karp polynomial rolling hash.

## 9. Invariant
At text pointer $i$, `j` characters of the pattern have already matched the suffix of the text ending at $i - 1$.

## 10. Dry Run
KMP matching `pat = "ABABC"` in text `"ABABABC"`:
| $i$ | $j$ | `text[i]` vs `pat[j]` | Action | Next $j$ |
|---|---|---|---|---|
| 0..3 | 0..3 | Matched `"ABAB"` | Advance $i, j$ | 4 |
| 4 | 4 | `'A'` vs `'C'` (Mismatch) | `j = lps[3] = 2` ($i$ stays 4) | 2 |
| 4 | 2 | `'A'` vs `'A'` (Match) | Advance $i, j$ | 3 |
| 5, 6 | 3, 4 | Matched `'B'`, `'C'` | Match complete! | Return index 2 |

## 11. Edge Cases
- Empty pattern: returns index 0 immediately.
- Pattern longer than text: returns -1 immediately.
- Highly periodic patterns (e.g. `"aaaa"`): correctly handles overlapping matches via `j = lps[j - 1]`.

## 12. Correctness
By definition of the LPS array: If a mismatch occurs at $j$, the longest prefix of $P[0..j-1]$ that is also a suffix is $P[0..lps[j-1]-1]$. Since this prefix is already aligned with the text ending at $i-1$, resetting $j = lps[j-1]$ preserves maximal alignment without missing any potential starting index.

## 13. Time Complexity
- Preprocessing: $O(M)$ time to build LPS table.
- Search: $O(N)$ time with strictly non-decreasing text pointer.
- Total: $O(N + M)$ deterministic linear time.

## 14. Space Complexity
- Auxiliary Space: $O(M)$ memory for LPS array.

## 15. Can It Be Optimized?
KMP is asymptotically optimal for single pattern search ($O(N + M)$). For searching multiple patterns simultaneously ($K$ patterns), the Aho-Corasick automaton searches in $O(N + \sum M_k)$ time.

## 16. When Should I Use This Algorithm?
- Single pattern substring matching in linear time.
- Longest Prefix that is also Suffix (LeetCode Longest Happy Prefix).
- Checking if a string is composed of repeated sub-patterns ($N \% (N - lps[N-1]) == 0$).
- Stream processing where text cannot be buffered or rewound.
- Palindromic substrings in linear time (Manacher's Algorithm).

## 17. When Should I NOT Use It?
- Short strings where Java's native `String.indexOf` (vectorized intrinsic) outperforms KMP in wall-clock time due to SIMD hardware instructions.
- Multiple patterns searching against static text (use Aho-Corasick or Trie).
- Fuzzy/approximate string matching with allowed edits (use Dynamic Programming).

---

## Comparison Table
| Algorithm | Time (Pre / Search) | Space | Features | Best Use |
|---|---|---|---|---|
| KMP | $O(M) / O(N)$ | $O(M)$ | Deterministic, no hash collisions | Exact substring search, periodic string analysis |
| Rabin-Karp | $O(M) / O(N)$ avg | $O(1)$ | Rolling polynomial hash | Multi-pattern search, 2D pattern matching, duplicate detection |
| Z-Algorithm | $O(N + M) / O(N + M)$ | $O(N + M)$ | Longest prefix match from every index | String concatenation tricks (`P + '$' + T`) |
| Manacher's | $O(N) / O(N)$ | $O(N)$ | Palindromic radii expansion | Longest palindromic substring in strictly $O(N)$ time |

---

### Algorithm: Knuth-Morris-Pratt (KMP)
- **Input / Output:** Text $T$ and Pattern $P$ $\to$ Start index of match.
- **Constraints:** $N \le 10^7, M \le 10^6$.
- **Brute Force:** Quadratic sliding scan ($O(N \cdot M)$).
- **Optimal Approach:** Build LPS table; fallback $j = lps[j-1]$ without resetting $i$.
- **How It Reduces Time/Space:** Never backtracks in text string.
- **Core Idea:** LPS array tracks self-similarity of pattern prefixes.
- **Pattern:** Prefix-suffix DFA matching.
- **Data Structure Used:** `int[] lps`.
- **Invariant:** Pattern prefix of length $j$ matches text suffix ending at $i-1$.
- **Dry Run:** Matches prefixes; on mismatch jumps to longest valid prefix boundary.
- **Edge Cases:** Pattern longer than text; empty pattern.
- **Correctness:** Follows from longest prefix-suffix preservation.
- **Time Complexity:** $O(N + M)$.
- **Space Complexity:** $O(M)$.
- **Can It Be Optimized:** Asymptotically optimal.
- **When to Use:** Exact pattern search without text rewind.
- **When NOT to Use:** Multi-pattern dictionaries (use Aho-Corasick).

### Algorithm: Rabin-Karp Algorithm
- **Input / Output:** Text $T$ and Pattern $P$ $\to$ Match indices.
- **Constraints:** $N \le 10^7, M \le 10^6$.
- **Brute Force:** Recalculate substring comparison in $O(M)$ at every index ($O(N \cdot M)$).
- **Optimal Approach:** Rolling polynomial hash: subtract exiting char, multiply by base, add entering char in $O(1)$.
- **How It Reduces Time/Space:** Recomputes window hash in $O(1)$ instead of $O(M)$.
- **Core Idea:** Equal strings must have equal hashes; verify characters only on hash collisions.
- **Pattern:** Rolling Polynomial Hash.
- **Data Structure Used:** `long` hash registers and modulo arithmetic.
- **Invariant:** At step $i$, window hash matches polynomial evaluation of $T[i..i+M-1]$.
- **Dry Run:** Computes hash of pattern; slides window across text updating hash in $O(1)$.
- **Edge Cases:** Hash collisions (use double hashing or verify with `equals`).
- **Correctness:** Polynomial hash bijection modulo large primes ($10^9 + 7$).
- **Time Complexity:** Average: $O(N + M)$; Worst: $O(N \cdot M)$ on adversarial collisions.
- **Space Complexity:** $O(1)$ auxiliary space.
- **Can It Be Optimized:** Fast NTT for general convolutions.
- **When to Use:** Multi-pattern search; detecting repeated subsegments; 2D matrix matching.
- **When NOT to Use:** Adversarial input environments where collisions cause $O(N \cdot M)$ worst case.

### Algorithm: Z-Algorithm
- **Input / Output:** String $S \to$ Array $Z$ where $Z[i]$ is longest common prefix between $S$ and $S[i..]$.
- **Constraints:** $|S| \le 10^7$.
- **Brute Force:** String comparison from each index ($O(N^2)$).
- **Optimal Approach:** Maintain active Z-box $[L, R]$; reuse precomputed $Z[i - L]$ values inside the box.
- **How It Reduces Time/Space:** Skips comparing characters inside previously matched $[L, R]$ interval.
- **Core Idea:** If $i < R$, $Z[i] \ge \min(R - i + 1, Z[i - L])$.
- **Pattern:** Z-Box interval reuse.
- **Data Structure Used:** `int[] Z`.
- **Invariant:** $[L, R]$ defines the farthest right-reaching substring matching a prefix of $S$.
- **Dry Run:** Constructs $Z$ array by copying inside box and expanding when $i + Z[i - L] \ge R$.
- **Edge Cases:** $i > R$ forces fresh linear scan.
- **Correctness:** Substring equivalence inside the Z-box.
- **Time Complexity:** $O(N)$.
- **Space Complexity:** $O(N)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Form pattern matching via $P + \text{'\$'} + T$; prefix periodicity.
- **When NOT to Use:** When $O(M)$ space is required instead of $O(N + M)$ (use KMP).

### Algorithm: Manacher's Algorithm
- **Input / Output:** String $S \to$ Longest palindromic substring or array of palindrome radii.
- **Constraints:** $|S| \le 10^7$.
- **Brute Force:** Expand around centers for every character ($O(N^2)$).
- **Optimal Approach:** Insert delimiters (`#`) to unify odd/even lengths; maintain rightmost palindrome $[L, R]$ and mirror radii.
- **How It Reduces Time/Space:** Palindrome symmetry around center $C$ allows copying radii from mirror index $2C - i$.
- **Core Idea:** A palindrome is symmetric; characters inside have identical radii to their mirrored counterparts.
- **Pattern:** Palindromic symmetry mirroring.
- **Data Structure Used:** `int[] P` radius array.
- **Invariant:** $[C - R, C + R]$ is the palindrome extending farthest to the right.
- **Dry Run:** Mirrors radii; expands beyond $R$ only when mirror reaches boundary.
- **Edge Cases:** Single character string; even length palindromes handled by `#` padding.
- **Correctness:** Symmetry of reflection across palindrome center.
- **Time Complexity:** $O(N)$ strictly linear time.
- **Space Complexity:** $O(N)$ transformed string and radius array.
- **Can It Be Optimized:** Mathematically optimal for all-palindromes discovery.
- **When to Use:** Longest Palindromic Substring in linear time; counting all palindromic substrings in $O(N)$.
- **When NOT to Use:** Short strings where expand-around-center $O(N^2)$ has lower constant factor.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic string algorithm problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [17 - Graph Algorithms](../17-graph-algorithms/README.md) | [19 - Bit Manipulation](../19-bit-manipulation/README.md)
