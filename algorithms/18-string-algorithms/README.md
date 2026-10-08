# String Algorithms

> High-performance pattern matching and string analysis: KMP, Rabin-Karp, Z-algorithm, and Manacher's.

## 1. Overview
String Algorithms provide sub-quadratic string processing for pattern matching, longest common substrings, and palindrome analysis. Classic algorithms like Knuth-Morris-Pratt (KMP) avoid redundant character backtracking by preprocessing the search pattern into a Longest Proper Prefix Suffix (LPS) array. Other essential techniques include Rabin-Karp polynomial rolling hashing, Z-algorithm, and Manacher's linear palindrome finder.

## 2. Time & Space Complexity
| Algorithm | Purpose | Preprocessing Time | Matching Time | Space |
| :--- | :--- | :--- | :--- | :--- |
| Knuth-Morris-Pratt (KMP) | Single pattern matching | O(m) | O(n) | O(m) |
| Rabin-Karp | Pattern matching / Multiple patterns | O(m) | O(n + m) avg, O(n*m) worst | O(1) |
| Z-Algorithm | Pattern matching / Prefix matches | O(n + m) | O(1) | O(n + m) |
| Manacher's Algorithm | Longest Palindromic Substring | O(n) | O(n) | O(n) |
| Aho-Corasick | Multi-pattern dictionary search | O(Total Pattern Length * Sigma) | O(n + matches) | O(Trie) |

KMP runs in strictly deterministic O(n + m) time with no risk of worst-case degeneration from hash collisions.

## 3. When to Use
- Finding occurrences of pattern string `P` within large text `T` in linear time.
- Longest palindromic substring in strictly O(n) time (Manacher's).
- Detecting repeated sub-patterns, string periods, or cyclic rotations.
- Multi-pattern search across DNA or streaming text (Rabin-Karp / Aho-Corasick).

## 4. When NOT to Use
- String length is very small (m, n <= 100, where Java's `indexOf()` has lower constant overhead).
- Simple single-character frequency or anagram checks (use frequency arrays).
- Prefix-only dictionary matching (a Trie is simpler and more flexible).

## 5. Why It Works
KMP precomputes the `lps` array: `lps[i]` is the length of the longest proper prefix of `pattern[0..i]` that is also a suffix of `pattern[0..i]`. Upon a character mismatch after matching k characters, the text pointer never backtracks; the pattern pointer merely shifts to `lps[k - 1]`.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Naive Matching) | Test pattern match starting at every text index | O(n * m) | O(1) |
| Optimized (KMP) | Skip known matching prefixes using LPS table | O(n + m) | O(m) |

KMP trades O(m) precomputation space for the LPS array to eliminate all text pointer backtracking.

## 7. Data Structures Used Here
- `int[] lps`: Longest Proper Prefix which is also Suffix array.
- `int[] z`: Z-array recording longest common prefix with string start.

## 8. Core Template (Java)
```java
// KMP LPS (Longest Prefix Suffix) Array Construction
int[] buildLPS(String pat) {
    int m = pat.length();
    int[] lps = new int[m];
    int len = 0, i = 1;
    while (i < m) {
        if (pat.charAt(i) == pat.charAt(len)) {
            len++;
            lps[i] = len;
            i++;
        } else if (len > 0) {
            len = lps[len - 1];
        } else {
            lps[i] = 0;
            i++;
        }
    }
    return lps;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Graph Algorithms](../17-graph-algorithms/README.md) | [Bit Manipulation](../19-bit-manipulation/README.md)
