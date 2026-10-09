# Linked List
> Linear collection of data elements where each element points to the next via reference pointers rather than contiguous memory slots.

## 1. Fundamentals
- What is it? A linear sequence of discrete node objects connected via forward (and optionally backward) pointer references.
- What problem does it solve? Eliminates expensive O(n) memory shifts and reallocations when inserting or deleting elements at arbitrary positions once a node reference is held.
- What type of data does it store? Generic payload elements wrapped in heap-allocated node containers.
- Linear or non-linear? Linear.
- Static or dynamic? Dynamic: nodes are allocated on the heap individually as needed and garbage collected when unlinked.
- Ordered or unordered? Ordered by pointer link traversal sequence from head to tail.
- Mutable or immutable? Mutable in Java: payload values and pointer references can be freely redirected.
- How is the data stored internally? Scattered non-contiguously throughout the JVM heap; cache locality is poor due to pointer chasing.
```text
Singly Linked List Memory Layout:
[head: 0x10]
    |
    v
+--------+------+       +--------+------+       +--------+------+
| val: 1 | 0x24 | ----> | val: 2 | 0x38 | ----> | val: 3 | null |
+--------+------+       +--------+------+       +--------+------+
Address: 0x10           Address: 0x24           Address: 0x38
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Allocate a new node holding the target data value.
  2. Point `newNode.next` to the successor node.
  3. Update predecessor node's `next` pointer to reference `newNode`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Inserting at the head is always O(1); inserting at index k requires traversing k nodes (O(n)).

### Delete
- **How it works:**
  1. Locate the predecessor of the target node to delete.
  2. Set `pred.next = targetNode.next` (bypassing the target node).
  3. Target node is orphaned and subsequently reclaimed by the Java garbage collector.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Deleting head is O(1); deleting by index or value requires O(n) traversal to find predecessor.

### Search
- **How it works:**
  1. Initialize pointer at head node.
  2. Step forward node-by-node comparing payload value against target.
  3. Return node or index when matched, or null/not-found when reaching end (`null`).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Searching requires sequential traversal; binary search cannot be performed efficiently on standard linked lists.

### Access
- **How it works:**
  1. Validate index is within range [0, size - 1].
  2. Start at head pointer and advance `next` reference `index` times.
  3. Return payload of the reached node.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Random access is not supported in O(1); reaching element at index k takes O(k) steps.

### Update
- **How it works:**
  1. Traverse from head to the target node at the given index.
  2. Overwrite `node.val = newVal`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Updating a known node reference is O(1); finding the node by index is O(n).

### Traverse
- **How it works:**
  1. Set pointer `curr = head`.
  2. While `curr != null`, visit `curr.val` and advance `curr = curr.next`.
  3. Terminate when `curr` becomes `null`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Visits all n nodes in sequential order with pointer dereference overhead.

### Sort
- **How it works:**
  1. Divide list into two halves using fast and slow pointer cycle detection/midpoint search.
  2. Recursively sort each half.
  3. Merge the two sorted linked lists by rewiring pointers without allocating extra nodes.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(log n) |
| Average | O(n log n) | O(log n) |
| Worst | O(n log n) | O(log n) |

Note: Merge sort is optimal for linked lists because pointer rewiring avoids the auxiliary array buffer required by array merge sort.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Head) | O(1) | O(1) | O(1) | O(1) |
| Insert (Arbitrary index) | O(1) | O(n) | O(n) | O(1) |
| Delete (Head) | O(1) | O(1) | O(1) | O(1) |
| Delete (Arbitrary index) | O(1) | O(n) | O(n) | O(1) |
| Search | O(1) | O(n) | O(n) | O(1) |
| Access | O(1) | O(n) | O(n) | O(1) |
| Update | O(1) | O(n) | O(n) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort (Merge Sort) | O(n log n) | O(n log n) | O(n log n) | O(log n) |

## 3. Variations
- **Standard version:** Singly Linked List (single `next` pointer per node). Trade-off: Low memory overhead per node, but cannot traverse backwards and deleting the tail node requires O(n) predecessor discovery even with a tail reference.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Doubly Linked List | Nodes contain both `next` and `prev` pointers | Bidirectional traversal; O(1) removal of known node | Higher memory per node (2 references) | Deques, LRU cache, browser history |
| Circular Singly Linked List | Last node's `next` points back to `head` | Continuous cyclic iteration without null checks | Risk of infinite loops if termination logic fails | Round-robin CPU schedulers, ring buffers |
| Circular Doubly Linked List | Head `prev` points to tail and tail `next` points to head | O(1) access to both ends; symmetric operations | Complex pointer updates on insertion/deletion | Fibonacci heaps, complex buffer allocators |
| Skip List | Multi-level hierarchy of forward pointers over sorted nodes | Probabilistic O(log n) search, insert, and delete | High memory overhead; randomized balance | Concurrent skip lists, Redis sorted sets |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Raw Pointer Links | Nodes store raw references; null marks list termination | No overhead on null checks | Minimal reference overhead |
| Sentinel / Dummy Nodes | Fixed empty pseudo-head and pseudo-tail nodes guard ends | Eliminates special-case branch logic for head/tail | 1 to 2 dummy node object allocations |
| Unrolled Linked List | Each node contains a small fixed-capacity array of elements | Reduces pointer dereferencing and improves cache | Slight waste in partially filled node arrays |

- **Java built-in equivalents:**
  - `java.util.LinkedList`: Doubly linked list implementing both `List` and `Deque` interfaces; backed by internal `Node<E>` objects containing `item`, `next`, and `prev`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyLinkedList.java` | Singly and Doubly Linked List | Node-based list with sentinel dummy node, insert, delete, and reverse |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [03 - Matrix & 2D Arrays](../03-matrix-2d-arrays/README.md) | [05 - Stack](../05-stack/README.md)
