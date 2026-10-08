# Design Problems

> Composite data structures orchestrating hash maps, linked lists, heaps, and trees for target APIs.

## 1. Overview
Design Problems require combining multiple foundational data structures to meet strict time and space complexity constraints across several API operations. Classic examples include LRU Cache (O(1) get/put using HashMap + Doubly Linked List), LFU Cache, and Min-Stack. The focus is on clean invariant maintenance, state synchronization, and corner-case encapsulation.

## 2. Time & Space Complexity
| Structure / Design | Target Operation | Target Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| LRU Cache | `get(key)`, `put(key, val)` | O(1) / O(1) / O(1) | O(Capacity) |
| LFU Cache | `get(key)`, `put(key, val)` | O(1) / O(1) / O(1) | O(Capacity) |
| Min Stack | `push(x)`, `pop()`, `getMin()` | O(1) / O(1) / O(1) | O(n) |
| RandomizedSet | `insert(val)`, `remove(val)`, `getRandom()` | O(1) / O(1) average | O(n) |
| Time-Based Key-Value Store | `set(k, v, t)`, `get(k, t)` | O(1) / O(log n) binary search | O(n) |

For RandomizedSet, O(1) deletion is achieved by swapping the target element with the last element in an ArrayList before popping the tail.

## 3. When to Use
- Problem specifies a multi-method class interface (e.g., `get`, `put`, `remove`, `getMin`).
- Every method demands sub-linear time constraints (e.g., O(1) or O(log n)).
- Need to maintain multiple ordering dimensions simultaneously (e.g., key lookup O(1) plus recency order O(1)).
- Simulating real-world systems like caching tiers, rate limiters, or hit counters.

## 4. When NOT to Use
- A single standard Java collection directly satisfies all operations without customization.
- Operations do not need strict worst-case or amortized O(1) performance guarantees.
- Problem is purely an algorithm on static input rather than an interactive stateful class.

## 5. Why It Works
Design patterns compose complementary structures: a hash map provides O(1) key indexing by referencing nodes, while a doubly linked list provides O(1) node detachment and insertion at head/tail. Because references point directly to the list node, finding and unlinking the node avoids linear scanning.

## 6. Brute Force vs Optimized
| Approach | Idea | `get` Time | `put` Time |
| :--- | :--- | :--- | :--- |
| Brute Force (ArrayList of Entries) | Linear scan to find key and update timestamp | O(n) | O(1) to append, O(n) to evict |
| Optimized (HashMap + Doubly Linked List) | Map maps key to Node; DLL maintains recency order | O(1) | O(1) |

The composite design trades dual-structure memory overhead (storing keys in both Map and DLL) to achieve O(1) for all operations.

## 7. Data Structures Used Here
- `HashMap<K, Node>`: Constant-time reference lookup.
- Doubly Linked List (`head` and `tail` sentinels): Constant-time node eviction and re-insertion.
- `ArrayList<E>` + `HashMap<E, Integer>`: For O(1) random retrieval and deletion via tail swap.

## 8. Core Template (Java)
```java
// LRU Cache core skeleton (HashMap + Doubly Linked List)
class LRUCache {
    static class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) { key = k; val = v; }
    }

    private final int capacity;
    private final Map<Integer, Node> map = new HashMap<>();
    private final Node head = new Node(0, 0);
    private final Node tail = new Node(0, 0);

    public LRUCache(int capacity) {
        this.capacity = capacity;
        head.next = tail;
        tail.prev = head;
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }

    private void addFirst(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    public int get(int key) {
        Node node = map.get(key);
        if (node == null) return -1;
        remove(node);
        addFirst(node);
        return node.val;
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Fenwick Tree (Binary Indexed Tree)](../16-fenwick-tree/README.md) | End
