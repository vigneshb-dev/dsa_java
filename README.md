# Data Structures & Algorithms in Java

A clean, beginner-friendly repository for studying Data Structures and Algorithms in Java, topic by topic.
Designed for disciplined practice, conceptual mastery, and structured interview preparation using plain Java.

## Quick Navigation

| Data Structures | Algorithms |
| :--- | :--- |
| [01 - Arrays](data-structures/01-arrays/) | [01 - Complexity Analysis](algorithms/01-complexity-analysis/) |
| [02 - Strings](data-structures/02-strings/) | [02 - Sorting](algorithms/02-sorting/) |
| [03 - Matrix & 2D Arrays](data-structures/03-matrix-2d-arrays/) | [03 - Searching](algorithms/03-searching/) |
| [04 - Linked List](data-structures/04-linked-list/) | [04 - Recursion](algorithms/04-recursion/) |
| [05 - Stack](data-structures/05-stack/) | [05 - Backtracking](algorithms/05-backtracking/) |
| [06 - Queue & Deque](data-structures/06-queue-deque/) | [06 - Divide and Conquer](algorithms/06-divide-and-conquer/) |
| [07 - Hashing](data-structures/07-hashing/) | [07 - Two Pointers](algorithms/07-two-pointers/) |
| [08 - Binary Tree](data-structures/08-binary-tree/) | [08 - Fast & Slow Pointers](algorithms/08-fast-slow-pointers/) |
| [09 - Binary Search Tree](data-structures/09-binary-search-tree/) | [09 - Sliding Window](algorithms/09-sliding-window/) |
| [10 - Balanced Trees](data-structures/10-balanced-trees/) | [10 - Prefix Sum & Difference Array](algorithms/10-prefix-sum-difference-array/) |
| [11 - Heap & Priority Queue](data-structures/11-heap-priority-queue/) | [11 - Monotonic Stack & Queue](algorithms/11-monotonic-stack-queue/) |
| [12 - Trie](data-structures/12-trie/) | [12 - Intervals](algorithms/12-intervals/) |
| [13 - Graph Representation](data-structures/13-graph-representation/) | [13 - Cyclic Sort](algorithms/13-cyclic-sort/) |
| [14 - Union-Find](data-structures/14-union-find/) | [14 - Greedy](algorithms/14-greedy/) |
| [15 - Segment Tree](data-structures/15-segment-tree/) | [15 - Dynamic Programming](algorithms/15-dynamic-programming/) |
| [16 - Fenwick Tree](data-structures/16-fenwick-tree/) | [16 - Tree Algorithms](algorithms/16-tree-algorithms/) |
| [17 - Design Problems](data-structures/17-design-problems/) | [17 - Graph Algorithms](algorithms/17-graph-algorithms/) |
| | [18 - String Algorithms](algorithms/18-string-algorithms/) |
| | [19 - Bit Manipulation](algorithms/19-bit-manipulation/) |
| | [20 - Math & Number Theory](algorithms/20-math-number-theory/) |
| | [21 - Matrix Algorithms](algorithms/21-matrix-algorithms/) |

## Repository Structure

```text
.
├── README.md
├── PROGRESS.md
├── .gitignore
├── notes/
├── data-structures/
│   ├── 01-arrays/
│   ├── 02-strings/
│   ├── 03-matrix-2d-arrays/
│   ├── 04-linked-list/
│   ├── 05-stack/
│   ├── 06-queue-deque/
│   ├── 07-hashing/
│   ├── 08-binary-tree/
│   ├── 09-binary-search-tree/
│   ├── 10-balanced-trees/
│   ├── 11-heap-priority-queue/
│   ├── 12-trie/
│   ├── 13-graph-representation/
│   ├── 14-union-find/
│   ├── 15-segment-tree/
│   ├── 16-fenwick-tree/
│   └── 17-design-problems/
└── algorithms/
    ├── 01-complexity-analysis/
    ├── 02-sorting/
    ├── 03-searching/
    ├── 04-recursion/
    ├── 05-backtracking/
    ├── 06-divide-and-conquer/
    ├── 07-two-pointers/
    ├── 08-fast-slow-pointers/
    ├── 09-sliding-window/
    ├── 10-prefix-sum-difference-array/
    ├── 11-monotonic-stack-queue/
    ├── 12-intervals/
    ├── 13-cyclic-sort/
    ├── 14-greedy/
    ├── 15-dynamic-programming/
    │   ├── 1d/
    │   ├── 2d-grid/
    │   ├── knapsack/
    │   ├── subsequences/
    │   ├── partition-and-interval/
    │   ├── bitmask/
    │   └── tree-dp/
    ├── 16-tree-algorithms/
    ├── 17-graph-algorithms/
    │   ├── traversal/
    │   ├── shortest-path/
    │   ├── minimum-spanning-tree/
    │   ├── topological-sort/
    │   ├── cycle-detection/
    │   ├── connected-components/
    │   └── bipartite/
    ├── 18-string-algorithms/
    ├── 19-bit-manipulation/
    ├── 20-math-number-theory/
    └── 21-matrix-algorithms/
```

## How to Use This Repo

1. **Where to add a new problem:**
   - Locate the relevant topic folder under `data-structures/` or `algorithms/`.
   - Place your file inside the appropriate difficulty subfolder (`easy/`, `medium/`, or `hard/`).
   - For `15-dynamic-programming` and `17-graph-algorithms`, place the file directly inside the target sub-topic folder (e.g., `algorithms/15-dynamic-programming/knapsack/`).

2. **Rule for choosing between `data-structures/` and `algorithms/`:**
   - **Put it where the main idea is.**
   - If the core breakthrough is implementing, manipulating, or choosing a specialized data structure (e.g., Trie, Disjoint Set, Min-Stack, LRU Cache, Binary Search Tree), place it under `data-structures/`.
   - If the core breakthrough is an algorithmic strategy or paradigm (e.g., Sliding Window, Binary Search on Answer, Dynamic Programming, Greedy, Two Pointers), place it under `algorithms/`.

3. **How to choose the difficulty subfolder:**
   - Follow the official platform difficulty tag (LeetCode / Codeforces / HackerRank).
   - Alternatively, categorize by conceptual depth:
     - `easy/`: Single-step operations, direct standard library usage, trivial loops.
     - `medium/`: Invariant tracking, multi-pointer coordinates, composite data transformations.
     - `hard/`: Subtle edge-case bounds, optimal state reductions, multi-layer algorithms.

4. **Updating progress:**
   - Make it a habit to update the topic's `README.md` problem table (marking `[x]`) and log your progress in [PROGRESS.md](PROGRESS.md) immediately after completing each problem.

## Naming Conventions

- **Problems:** `P<4-digit-number>_PascalCaseTitle.java`  
  *Example:* `P0003_LongestSubstringWithoutRepeating.java`
- **Own implementations:** `My<Name>.java`  
  *Example:* `MyMinHeap.java`
- **Templates:** `<Name>Template.java`  
  *Example:* `BinarySearchTemplate.java`
- **Class Name:** The public class name must match the file name exactly.
- **No Package Declarations:** Keep files as plain Java (`.java`) without `package` statements for immediate compilation.

## File Template

```java
/**
 * Problem: P0003 - Longest Substring Without Repeating Characters
 * Difficulty: Medium
 * Pattern: Sliding Window
 * Link: https://leetcode.com/problems/longest-substring-without-repeating-characters/
 *
 * Idea:
 * - Maintain a dynamic sliding window [left, right] storing the last seen index of each character.
 * - When a duplicate character is encountered, jump the left pointer forward past its previous index.
 *
 * Time Complexity: O(n) - each character is visited at most twice.
 * Space Complexity: O(min(m, n)) - bounded by alphabet size and string length.
 *
 * Mistakes:
 * - Remember to update left pointer using Math.max(left, map[ch] + 1) to avoid moving backward.
 */
public class P0003_LongestSubstringWithoutRepeating {

    public static int lengthOfLongestSubstring(String s) {
        int[] last = new int[128];
        java.util.Arrays.fill(last, -1);
        int maxLen = 0, left = 0;

        for (int right = 0; right < s.length(); right++) {
            char c = s.charAt(right);
            if (last[c] >= left) {
                left = last[c] + 1;
            }
            last[c] = right;
            maxLen = Math.max(maxLen, right - left + 1);
        }
        return maxLen;
    }

    public static void main(String[] args) {
        String test = "abcabcbb";
        int result = lengthOfLongestSubstring(test);
        System.out.println("Result: " + result); // Expected: 3
    }
}
```

## How to Run a File

From inside the terminal, navigate to the folder containing your file and execute:

```bash
# 1. Navigate to the folder:
cd algorithms/09-sliding-window/medium

# 2. Compile the Java file:
javac P0003_LongestSubstringWithoutRepeating.java

# 3. Run the compiled bytecode:
java P0003_LongestSubstringWithoutRepeating
```

## Suggested Study Order

Follow the number prefixes across data structures and algorithms. The numbering is structured to build foundational primitives before progressing to advanced paradigms:

1. **Foundations:** `algorithms/01-complexity-analysis` &rarr; `data-structures/01-arrays` &rarr; `data-structures/02-strings`
2. **Sequential Patterns:** `algorithms/07-two-pointers` &rarr; `algorithms/08-fast-slow-pointers` &rarr; `algorithms/09-sliding-window` &rarr; `algorithms/10-prefix-sum-difference-array`
3. **Linear Collections:** `data-structures/04-linked-list` &rarr; `data-structures/05-stack` &rarr; `data-structures/06-queue-deque` &rarr; `algorithms/11-monotonic-stack-queue`
4. **Ordering & Search:** `algorithms/02-sorting` &rarr; `algorithms/03-searching` &rarr; `algorithms/12-intervals` &rarr; `algorithms/13-cyclic-sort`
5. **Recursion & Combinatorics:** `algorithms/04-recursion` &rarr; `algorithms/05-backtracking` &rarr; `algorithms/06-divide-and-conquer`
6. **Hierarchical & Associative:** `data-structures/07-hashing` &rarr; `data-structures/08-binary-tree` &rarr; `data-structures/09-binary-search-tree` &rarr; `data-structures/11-heap-priority-queue` &rarr; `data-structures/12-trie`
7. **Optimization Paradigms:** `algorithms/14-greedy` &rarr; `algorithms/15-dynamic-programming` &rarr; `data-structures/13-graph-representation` &rarr; `data-structures/14-union-find` &rarr; `algorithms/17-graph-algorithms`
8. **Specialized & Range Structures:** `data-structures/15-segment-tree` &rarr; `data-structures/16-fenwick-tree` &rarr; `algorithms/18-string-algorithms` &rarr; `algorithms/19-bit-manipulation` &rarr; `algorithms/20-math-number-theory` &rarr; `data-structures/17-design-problems`

*(This suggested sequence can be adapted freely to match personal interview timelines or study goals).*

## Commit Message Convention

Maintain a clean, searchable git history using standard prefixes:

- `solve: LC 3 sliding window` &mdash; solved a problem
- `notes: add binary search template` &mdash; added or updated notes/templates
- `revise: LC 15` &mdash; re-solved or refactored a previously completed problem
- `impl: MyMinHeap` &mdash; implemented a custom data structure

## Progress

Track your overall problem count, topic confidence, revision goals, and weekly logs in [PROGRESS.md](PROGRESS.md).
