# Queue & Deque

> First-In-First-Out (FIFO) queues and double-ended queues (Deque) for order-preserving processing.

## 1. Overview
A Queue is a linear collection operating under First-In-First-Out (FIFO) semantics, where items enter at the rear and depart from the front. A Deque (Double-Ended Queue) generalizes this by allowing O(1) insertions and deletions at both the front and the rear. In Java, `ArrayDeque` is the standard high-performance implementation for both queues and deques.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `offer()` / `addLast()` | O(1) / O(1) / O(1) amortized | O(1) |
| `poll()` / `pollFirst()` | O(1) / O(1) / O(1) | O(1) |
| `peek()` / `peekFirst()` | O(1) / O(1) / O(1) | O(1) |
| `addFirst()` / `pollLast()` | O(1) / O(1) / O(1) | O(1) |
| `size()` / `isEmpty()` | O(1) / O(1) / O(1) | O(1) |

ArrayDeque operations are amortized O(1); power-of-two capacity sizing and bitwise masking provide efficient circular buffer indexing.

## 3. When to Use
- Breadth-First Search (BFS) level-order tree and graph traversals.
- Task scheduling, job queues, or rate limiting buffers.
- Sliding window maximum/minimum algorithms requiring double-ended removals.
- Palindrome verification comparing characters from both ends concurrently.

## 4. When NOT to Use
- Items must be processed by priority rather than arrival time (use `PriorityQueue`).
- LIFO access is desired exclusively (use stack methods on Deque).
- Arbitrary index lookup is required (use `ArrayList`).

## 5. Why It Works
FIFO ordering guarantees that nodes at distance `d` from the source in an unweighted graph are explored before any node at distance `d + 1`. A circular ring buffer uses head and tail indices modulo capacity to achieve constant-time push and pop without shifting elements.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (ArrayList as Queue) | Add at end, remove index 0 (shifts all elements) | O(n^2) for n ops | O(n) |
| Optimized (ArrayDeque) | Circular array buffer head and tail pointer movement | O(n) for n ops | O(n) |

ArrayDeque maintains circular head/tail pointers, turning O(n) shift removals into O(1) index updates.

## 7. Data Structures Used Here
- `ArrayDeque<E>`: Circular array implementation of `Deque` and `Queue`.
- `LinkedList<E>`: Doubly-linked list implementation of `Queue` and `Deque` (carries node pointer overhead).

## 8. Core Template (Java)
```java
// Standard Queue (FIFO) usage via ArrayDeque
Queue<Integer> queue = new ArrayDeque<>();
queue.offer(1);
queue.offer(2);
while (!queue.isEmpty()) {
    int curr = queue.poll();
    // Process curr
}

// Deque (Double-Ended) usage
Deque<Integer> deque = new ArrayDeque<>();
deque.addFirst(10);
deque.addLast(20);
int first = deque.pollFirst();
int last = deque.pollLast();
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Stack](../05-stack/README.md) | [Hashing](../07-hashing/README.md)
