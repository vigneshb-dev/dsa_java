# Strings

> Immutable character sequences in Java requiring builder buffers for efficient mutation.

## 1. Overview
A String is an indexed sequence of characters representing textual data. In Java, `java.lang.String` instances are immutable and backed by an internal compact byte array with an encoding coder flag. Because strings are immutable, modifying a string creates a new object unless mutable buffers like `StringBuilder` are employed.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `charAt(i)` | O(1) / O(1) / O(1) | O(1) |
| `length()` | O(1) / O(1) / O(1) | O(1) |
| `substring(i, j)` | O(k) / O(k) / O(k) where k = j - i | O(k) |
| String Concatenation (`+` in loop) | O(n) / O(n^2) / O(n^2) total | O(n^2) |
| `StringBuilder.append()` | O(1) / O(1) amortized / O(n) worst | O(1) |
| `equals()` | O(1) / O(n) / O(n) | O(1) |

In modern Java (Java 9+), `substring` allocates a copy of the byte sub-sequence, making it O(k) time and space rather than O(1).

## 3. When to Use
- Text processing, tokenization, or anagram/palindrome verification.
- Substrings or pattern matching problems over a fixed alphabet.
- Prefix or suffix querying where trie or hashing structures can be formed.
- When string immutability provides thread-safety and hash code caching benefits.

## 4. When NOT to Use
- Frequent in-place character mutation in high-performance loops (use `char[]` or `StringBuilder` instead).
- Streaming huge documents exceeding heap memory (use I/O streaming or memory-mapped buffers).
- Complex grammar parsing requiring AST generators instead of primitive string manipulation.

## 5. Why It Works
Strings maintain sequential indices where character frequency arrays (e.g., `int[26]` or `int[128]`) can act as constant-space hash maps. Comparing two anagrams becomes an O(n) count comparison because character permutations preserve frequency multiset invariants.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Concatenation) | Repeated `s += ch` inside a loop | O(n^2) | O(n) |
| Optimized (StringBuilder / char[]) | Append characters into resizable buffer | O(n) | O(n) |

The optimized approach avoids allocating n intermediate string objects by mutating a single contiguous buffer.

## 7. Data Structures Used Here
- `String`: Immutable sequence backed by `byte[]` and `coder` byte in Java 9+.
- `StringBuilder`: Non-synchronized mutable character sequence backed by resizable `byte[]` buffer.
- `char[]`: Direct primitive character array providing zero-overhead index mutations.

## 8. Core Template (Java)
```java
// Frequency array for ASCII lowercase strings
String s = "example";
int[] freq = new int[26];
for (int i = 0; i < s.length(); i++) {
    freq[s.charAt(i) - 'a']++;
}

// Efficient mutable string building
StringBuilder sb = new StringBuilder();
for (char c : s.toCharArray()) {
    sb.append(c);
}
String result = sb.toString();
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Arrays](../01-arrays/README.md) | [Matrix & 2D Arrays](../03-matrix-2d-arrays/README.md)
