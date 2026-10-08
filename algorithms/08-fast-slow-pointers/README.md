# Fast & Slow Pointers

> Pointers moving at different speeds (Floyd's Tortoise and Hare) to detect cycles and midpoints.

## 1. Overview
Fast and Slow Pointers (also known as Floyd's Tortoise and Hare algorithm) employs two pointers that traverse a sequence or linked structure at different speeds (typically 1 step vs 2 steps per iteration). This technique detects cycles in linked lists, finds the entry point of a loop, and locates the exact midpoint of a list in a single pass without prior length calculation. It requires strictly O(1) extra space, eliminating the need for visited hash sets.

## 2. Time & Space Complexity
| Application / Variant | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| Detect Cycle in Linked List | O(n) | O(1) |
| Find Cycle Start Node | O(n) | O(1) |
| Find Middle of Linked List | O(n) | O(1) |
| Happy Number (Digit square sum cycle) | O(log n) | O(1) |
| Find Duplicate Number (Array as linked list) | O(n) | O(1) |

If a cycle exists of length C, once both pointers enter the cycle, the relative speed difference of 1 node/step guarantees they meet within C iterations.

## 3. When to Use
- Detecting loops or cycles in linked lists or state transitions.
- Locating the start node of a linked list cycle.
- Finding the middle node of a linked list in one pass (e.g., for merge sort or palindrome checking).
- Detecting infinite cycle repetition in iterative number sequences (Happy Number).

## 4. When NOT to Use
- Data structure provides O(1) random index access (length / 2 directly accesses array midpoint).
- Cycles can branch into general graphs (use DFS cycle detection with visited sets).
- Multiple distinct cycles exist across disconnected components.

## 5. Why It Works
Let the distance from head to cycle entrance be `F`, and distance from entrance to meeting point be `a`. When slow moves `F + a`, fast moves `2(F + a) = F + a + k*C`. Simplifying gives `F = k*C - a`. Thus, resetting one pointer to the head and advancing both at 1 step/iteration ensures they meet precisely at the cycle entrance.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Visited Hash Set | Store visited node references in `HashSet<ListNode>` | O(n) | O(n) |
| Floyd's Cycle Finding | Slow moves 1 step, fast moves 2 steps; compare references | O(n) | O(1) |

Floyd's algorithm trades pointer arithmetic to eliminate hash table memory overhead down to O(1).

## 7. Data Structures Used Here
- `ListNode`: Singly linked list nodes containing `next` pointer references.

## 8. Core Template (Java)
```java
// Cycle detection and midpoint skeleton
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) {
            return true; // cycle detected
        }
    }
    return false; // reached end of list
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Two Pointers](../07-two-pointers/README.md) | [Sliding Window](../09-sliding-window/README.md)
