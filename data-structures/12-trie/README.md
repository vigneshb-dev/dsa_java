# Trie

> Prefix tree where edges represent characters, enabling fast retrieval and prefix searches.

## 1. Overview
A Trie (pronounced 'try'), or prefix tree, is an ordered tree structure used to store an associative collection of strings. Unlike a binary tree, keys are not stored directly in nodes; instead, a node's position within the tree defines its associated key prefix. All descendants of a node share a common prefix, making prefix lookups proportional to word length rather than dictionary size.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Insert Word of length L | O(L) / O(L) / O(L) | O(L * Sigma) |
| Search Word of length L | O(L) / O(L) / O(L) | O(1) |
| StartsWith Prefix of length P | O(P) / O(P) / O(P) | O(1) |
| Delete Word of length L | O(L) / O(L) / O(L) | O(L) recursion |

Sigma denotes alphabet size (e.g., 26 for lowercase English letters). Time complexity is independent of the number of words N in the trie.

## 3. When to Use
- Autocomplete suggestions and predictive typing systems.
- Prefix-matching queries (`startsWith`) over large string dictionaries.
- Spell checkers and dictionary validation.
- Bitwise Trie for Maximum XOR queries of two numbers in an array.

## 4. When NOT to Use
- Only exact whole-word lookups are needed without prefix queries (a `HashSet<String>` is simpler and uses less memory).
- Alphabet size Sigma is massive (e.g., Unicode) without sparse child maps, leading to high memory overhead.
- Keys are arbitrary non-sequence data types.

## 5. Why It Works
Strings with identical prefixes share common ancestral branches, eliminating redundant character comparisons. Traversing each character takes O(1) child array index lookup (`c - 'a'`), leading to deterministic O(L) query time.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Array of Strings) | Scan all N words and call `startsWith()` on each | O(N * L) | O(1) |
| Optimized (Trie Search) | Walk down prefix branch directly | O(L) | O(Total characters * Sigma) |

The Trie trades additional node pointer memory to decouple lookup runtime from dictionary size N.

## 7. Data Structures Used Here
- `TrieNode`: Custom class holding `TrieNode[] children = new TrieNode[26]` and `boolean isEndOfWord`.
- `Map<Character, TrieNode>`: Alternative child representation for sparse or arbitrary alphabets.

## 8. Core Template (Java)
```java
// Standard Trie node and operations skeleton
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEnd = false;
}

class Trie {
    private final TrieNode root = new TrieNode();

    public void insert(String word) {
        TrieNode curr = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) {
                curr.children[idx] = new TrieNode();
            }
            curr = curr.children[idx];
        }
        curr.isEnd = true;
    }

    public boolean startsWith(String prefix) {
        TrieNode curr = root;
        for (char c : prefix.toCharArray()) {
            int idx = c - 'a';
            if (curr.children[idx] == null) return false;
            curr = curr.children[idx];
        }
        return true;
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Heap & Priority Queue](../11-heap-priority-queue/README.md) | [Graph Representation](../13-graph-representation/README.md)
