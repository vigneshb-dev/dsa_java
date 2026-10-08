# Balanced Trees

> Self-balancing search trees (AVL, Red-Black) guaranteeing O(log n) worst-case operations.

## 1. Overview
Balanced Trees are binary search trees augmented with balancing rules and tree rotation mechanisms to maintain height bounded by O(log n). Popular variants include AVL trees (strictly balanced with height difference <= 1) and Red-Black trees (color-based balancing). In Java, `TreeMap` and `TreeSet` are built upon self-balancing Red-Black trees.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Search | O(1) / O(log n) / O(log n) | O(1) |
| Insert | O(1) / O(log n) / O(log n) | O(1) |
| Delete | O(1) / O(log n) / O(log n) | O(1) |
| Floor / Ceiling | O(1) / O(log n) / O(log n) | O(1) |
| Min / Max | O(1) / O(log n) / O(log n) | O(1) |

A Red-Black tree bounds maximum height to 2 * log2(n + 1), guaranteeing strictly logarithmic performance for all operations.

## 3. When to Use
- Guaranteed O(log n) search, insertion, and deletion regardless of input order.
- Dynamic range queries, floor, ceiling, predecessor, or successor operations.
- Maintaining a live sorted stream of elements with frequent insertions and deletions.
- Implementing order-statistic trees or interval trees.

## 4. When NOT to Use
- Data is static and known in advance (a sorted array with binary search has less overhead).
- Only exact equality lookups are required (a `HashMap` is faster on average with O(1)).
- Strict memory constraints where node references and color/balance metadata are prohibitive.

## 5. Why It Works
Tree rotations (left and right) restructure local parent-child links in O(1) time without violating BST ordering. By re-coloring nodes and performing at most two rotations on insertion (or three on deletion), the tree height invariant is preserved.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Unbalanced BST | Insert keys without rotations; degrades on sorted inputs | O(n) worst | O(n) |
| Balanced BST (Red-Black) | Rotate and recolor upon violations to keep height O(log n) | O(log n) worst | O(n) |

Rotations trade a small constant re-linking cost during mutation to protect against worst-case linear O(n) degeneration.

## 7. Data Structures Used Here
- `TreeMap<K, V>`: Java standard library NavigableMap backed by a Red-Black Tree.
- `TreeSet<E>`: NavigableSet backed by a `TreeMap`.
- Custom `AVLNode` / `RBNode`: Hand-crafted tree nodes with height or color metadata.

## 8. Core Template (Java)
```java
// Using Java's built-in Red-Black tree via TreeMap
TreeMap<Integer, String> map = new TreeMap<>();
map.put(20, "Twenty");
map.put(10, "Ten");
map.put(30, "Thirty");

// O(log n) range operations
Integer floorKey = map.floorKey(25);     // 20 (greatest <= 25)
Integer ceilingKey = map.ceilingKey(25); // 30 (least >= 25)
int firstKey = map.firstKey();          // 10 (minimum)
int lastKey = map.lastKey();            // 30 (maximum)
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Binary Search Tree](../09-binary-search-tree/README.md) | [Heap & Priority Queue](../11-heap-priority-queue/README.md)
