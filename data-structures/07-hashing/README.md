# Hashing

> Key-value mapping and uniqueness tracking with expected O(1) average-time operations.

## 1. Overview
Hashing maps keys to integer bucket indices via a hash function, enabling expected constant-time lookups, insertions, and deletions. Java's `HashMap` handles collisions through separate chaining with linked lists that convert to balanced red-black trees when bucket size reaches 8. Hash-based structures trade memory overhead for high-speed associative querying.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `put(k, v)` | O(1) / O(1) / O(n) (O(log n) in Java 8+) | O(1) |
| `get(k)` | O(1) / O(1) / O(n) (O(log n) in Java 8+) | O(1) |
| `containsKey(k)` | O(1) / O(1) / O(n) (O(log n) in Java 8+) | O(1) |
| `remove(k)` | O(1) / O(1) / O(n) (O(log n) in Java 8+) | O(1) |
| Iteration over entries | O(n + capacity) / O(n + capacity) | O(1) |

Java 8+ treeifies bins when bucket count >= 8 and table capacity >= 64, bounding worst-case lookup to O(log n) even under severe hash collisions.

## 3. When to Use
- Immediate O(1) lookup of values associated with unique keys.
- Tracking element frequencies, duplicates, or visited states.
- Two Sum problem variants checking complement existence in one pass.
- Grouping items by an anagram signature or normalized key.

## 4. When NOT to Use
- Data must remain sorted or range queries (min, max, floor, ceiling) are needed (use `TreeMap`).
- Keys are dense small integers from 0 to N (use primitive array `int[]` for faster zero-allocation indexing).
- Order of insertion must be preserved without extra overhead (use `LinkedHashMap`).

## 5. Why It Works
A well-distributed hash function uniformly spreads keys across buckets so average bucket length remains a small constant. By computing `index = (n - 1) & hash(key)`, the bucket is directly accessed in O(1) memory lookup time.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Nested Loop) | Check every pair `(i, j)` to test if sum equals target | O(n^2) | O(1) |
| Optimized (Hash Table Complement) | Store visited elements in HashMap; query `target - x` | O(n) | O(n) |

The hash table trades O(n) auxiliary space to drop lookup time for complements from O(n) to O(1).

## 7. Data Structures Used Here
- `HashMap<K, V>`: Hash table bucket array with linked lists / red-black trees.
- `HashSet<E>`: Set backed internally by a `HashMap` where values are dummy objects.
- `LinkedHashMap<K, V>`: HashMap with a doubly linked list running through all entries to preserve insertion or access order.

## 8. Core Template (Java)
```java
// Frequency counting and lookup template
Map<Integer, Integer> counts = new HashMap<>();
int[] nums = {1, 2, 2, 3};
for (int x : nums) {
    counts.put(x, counts.getOrDefault(x, 0) + 1);
}

// Membership check via HashSet
Set<Integer> seen = new HashSet<>();
for (int x : nums) {
    if (seen.contains(x)) {
        // Duplicate detected
    }
    seen.add(x);
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Queue & Deque](../06-queue-deque/README.md) | [Binary Tree](../08-binary-tree/README.md)
