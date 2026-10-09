# Trie
> Prefix tree data structure optimizing string retrieval, prefix matching, and lexicographical search.

## 1. Fundamentals
- What is it? A tree-based search structure where edges represent characters and each path from the root corresponds to a common string prefix.
- What problem does it solve? Enables $O(L)$ prefix lookups, autocomplete suggestions, spell-checking, and longest-prefix matching independent of dictionary size $N$.
- What type of data does it store? Strings, character sequences, or bit arrays.
- Linear or non-linear? Non-linear (multi-way hierarchical tree).
- Static or dynamic? Dynamic: allocates new branch nodes as unique prefixes are inserted.
- Ordered or unordered? Strictly ordered: pre-order depth-first traversal enumerates stored words in lexicographical order.
- Mutable or immutable? Mutable in Java: words and branch links can be inserted and pruned dynamically.
- How is the data stored internally? Composed of `TrieNode` objects containing a table of child references (array of size $\Sigma$ or hash map) and a boolean `isEndOfWord` marker.
```text
Trie (Prefix Tree) Internal Layout:
                 (Root)
                /      \
             ['c']    ['t']
             /          \
           ['a']       ['o']
           /   \         \
       ['t']* ['r']*    ['p']*
       ("cat") ("car")  ("top")  (* marks word end)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Start at the root node and iterate through the characters of string $W$ of length $L$.
  2. For character $c$, check if child link exists; if null, allocate and link a new `TrieNode`.
  3. Advance to the child node. After processing all $L$ characters, set `curr.isEndOfWord = true`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(L) | O(1) |
| Average | O(L) | O(L) |
| Worst | O(L) | O(L * $\Sigma$) |

Note: Time is strictly proportional to string length $L$; space is $O(1)$ if the prefix already exists in the trie.

### Delete
- **How it works:**
  1. Recursively traverse the branch for word of length $L$.
  2. Unset `isEndOfWord = false` at the terminal node.
  3. On unwinding recursion, prune and de-reference nodes that have no other children and are not marked as end-of-word.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(L) | O(L) |
| Average | O(L) | O(L) |
| Worst | O(L) | O(L) |

Note: Recursion call-stack depth is equal to word length $L$.

### Search
- **How it works:**
  1. Traverse down from root matching characters of key.
  2. If at any step child link is null, key does not exist; return false.
  3. After consuming all $L$ characters, return `curr.isEndOfWord`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(L) | O(1) |
| Worst | O(L) | O(1) |

Note: Search speed depends entirely on key length $L$, completely independent of the total number of words $N$.

### Access
- **How it works:**
  1. Often named `startsWith(prefix)`.
  2. Follow character links down from root for prefix of length $L$.
  3. If child link is missing, return false; otherwise return true if path exists.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(L) | O(1) |
| Worst | O(L) | O(1) |

Note: Prefix lookup does not require checking the `isEndOfWord` flag.

### Update
- **How it works:**
  1. In-place character modification inside a branch is not supported.
  2. Perform deletion of the old string followed by insertion of the replacement string.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(L) | O(L) |
| Average | O(L) | O(L) |
| Worst | O(L) | O(L) |

Note: Replaces string in $O(L_{old} + L_{new})$ operations.

### Traverse
- **How it works:**
  1. Perform Depth-First Search (DFS) from the root.
  2. Maintain a running string buffer of edge characters.
  3. Collect words whenever `isEndOfWord == true`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(T) | O(L_{max}) |
| Average | O(T) | O(L_{max}) |
| Worst | O(T) | O(L_{max}) |

Note: $T$ is the total number of nodes in the trie; auxiliary space is bounded by maximum word length $L_{max}$.

### Sort
- **How it works:**
  1. DFS traversal from root naturally visits children in alphabetical order (e.g. 'a' through 'z').
  2. Outputs all stored words in sorted lexicographical order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(T) | O(L_{max}) |
| Average | O(T) | O(L_{max}) |
| Worst | O(T) | O(L_{max}) |

Note: Performs lexicographical sort in linear time relative to total character count.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert | O(L) | O(L) | O(L) | O(L) |
| Delete | O(L) | O(L) | O(L) | O(L) |
| Search (Exact) | O(1) | O(L) | O(L) | O(1) |
| Access (Prefix) | O(1) | O(L) | O(L) | O(1) |
| Update | O(L) | O(L) | O(L) | O(L) |
| Traverse | O(T) | O(T) | O(T) | O(L_{max}) |
| Sort (Lexicographical)| O(T) | O(T) | O(T) | O(L_{max}) |

## 3. Variations
- **Standard version:** Fixed Array Trie (`TrieNode[26]`). Trade-off: Blazing fast array index lookups (`c - 'a'`), but significant memory wasted on null pointers when nodes have few branches.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Radix Tree / Patricia Trie | Compresses single-child chains into a single edge | Drastic reduction in node count and depth | Complex edge splitting and merging logic | IP routing tables, compressed memory stores |
| Ternary Search Tree (TST) | Each node has 3 children (lower, equal, higher) | Memory comparable to binary search tree; supports Unicode | Lookups take O(L + log $\Sigma$) instead of pure O(L) | Large alphabets, spellcheck dictionaries |
| Bitwise / Binary Trie | Alphabet is binary digits {0, 1} | O(32) or O(64) bitwise operations | Fixed 32 or 64 level depth | Maximum XOR pair queries, IP CIDR lookup |
| Suffix Tree | Stores all suffixes of a single string | Solves longest common substring in O(n) | High construction complexity (Ukkonen's algorithm) | Bioinformatics, DNA sequence matching |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Fixed Array (`TrieNode[26]`) | Child pointers indexed directly via `c - 'a'` | Pure O(1) pointer transition per char | 26 pointers per node ($\approx$ 208 bytes even if mostly null) |
| Hash Map (`Map<Character, Node>`) | Child pointers stored in dynamic bucket map | Hash computation and collision resolution overhead | Compact: only allocates references for existing edges |
| Ternary Links (`left, mid, right`) | Binary search tree over alphabet at each level | Logarithmic child branching | Exactly 3 child references per node |

- **Java built-in equivalents:**
  - Standard Java has no built-in `Trie` class in `java.util`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyTrie.java` | Standard 26-way Trie & Bitwise Trie | Standard prefix tree with insert, search, startsWith, and bitwise XOR query |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [11 - Heap & Priority Queue](../11-heap-priority-queue/README.md) | [13 - Graph Representation](../13-graph-representation/README.md)
