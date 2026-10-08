# Linked List

> Non-contiguous linear collections linked by pointers allowing O(1) insertion and deletion at known nodes.

## 1. Overview
A Linked List is a linear data structure where elements (nodes) are stored arbitrarily in memory and chained together via pointers. Each node encapsulates a data payload and one or more references (`next`, `prev`) pointing to neighboring nodes. They support constant time insertion and deletion once a pointer to the target predecessor node is already held.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Access / Search by Index | O(1) / O(n) / O(n) | O(1) |
| Insert at Head | O(1) / O(1) / O(1) | O(1) |
| Insert / Delete after known node | O(1) / O(1) / O(1) | O(1) |
| Insert at End (with tail pointer) | O(1) / O(1) / O(1) | O(1) |
| Delete at End (singly linked, with tail) | O(n) / O(n) / O(n) | O(1) |
| Delete at End (doubly linked, with tail) | O(1) / O(1) / O(1) | O(1) |

Deleting the tail in a singly linked list requires traversing to the second-to-last node (O(n)), whereas a doubly linked list has `tail.prev` for O(1) deletion.

## 3. When to Use
- Frequent insertions and deletions at the head or known positions without shifting subsequent elements.
- Implementing queue, deque, or stack primitives where node allocation on demand is acceptable.
- Building composite structures like LRU Cache (`LinkedHashMap` combining hash map + doubly linked list).
- Memory allocations occur in fragmented chunks where large contiguous blocks cannot be acquired.

## 4. When NOT to Use
- Random index access is needed frequently (arrays provide O(1) vs linked list O(n)).
- High performance memory and cache locality are critical (node pointer overhead and cache misses hurt performance).
- Binary search is required (binary search is ineffective on standard linked lists).

## 5. Why It Works
Pointers decouple logical ordering from physical memory location. Using a dummy/sentinel node (`ListNode dummy = new ListNode(0, head)`) eliminates special-case handling for inserting or deleting the head node.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Reverse via Array) | Copy values to array, reverse array, overwrite list | O(n) | O(n) |
| Optimized (In-Place Pointer Reversal) | Maintain prev, curr, next pointers and flip links | O(n) | O(1) |

In-place pointer manipulation eliminates auxiliary memory by directly rewiring reference pointers.

## 7. Data Structures Used Here
- `ListNode`: Custom singly/doubly linked node containing `val`, `next`, (and optional `prev`).
- `LinkedList<E>`: Java standard library doubly-linked list implementing `List` and `Deque` interfaces.

## 8. Core Template (Java)
```java
// Standard singly linked list node definition and dummy-head pattern
class ListNode {
    int val;
    ListNode next;
    ListNode(int val) { this.val = val; }
    ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

// Reversal skeleton
ListNode prev = null;
ListNode curr = head;
while (curr != null) {
    ListNode nextNode = curr.next;
    curr.next = prev;
    prev = curr;
    curr = nextNode;
}
// prev is now the new head
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Matrix & 2D Arrays](../03-matrix-2d-arrays/README.md) | [Stack](../05-stack/README.md)
