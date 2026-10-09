# Queue & Deque
> Linear data structures supporting First-In, First-Out (FIFO) access or double-ended insertion and deletion.

## 1. Fundamentals
- What is it? A Queue is a FIFO collection where elements enter at the rear and exit from the front; a Deque (Double-Ended Queue) permits insertion and removal at both ends.
- What problem does it solve? Regulates rate-limiting buffers, breadth-first search (BFS) level exploration, asynchronous task queues, and sliding-window boundary tracking.
- What type of data does it store? Generic payload elements (primitives or object references).
- Linear or non-linear? Linear.
- Static or dynamic? Static in fixed ring buffers; dynamic in resizable circular arrays (`ArrayDeque`) and linked lists (`LinkedList`).
- Ordered or unordered? Ordered by arrival sequence (temporal FIFO order for Queue).
- Mutable or immutable? Mutable in Java: states update dynamically as elements are enqueued and dequeued.
- How is the data stored internally? Typically implemented as a circular array buffer where `head` and `tail` indices wrap around using modulo arithmetic (`(index + 1) % capacity`), or via a doubly linked list.
```text
Circular Array Ring Buffer Internal Layout:
         head                    tail
           |                       |
           v                       v
       +-------+-------+-------+-------+-------+
idx:   |   0   |   1   |   2   |   3   |   4   |
val:   |  10   |  20   |  30   | null  | null  |
       +-------+-------+-------+-------+-------+
       <------ active elements -------->
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `offer(x)`, `enqueue(x)`, `addLast(x)`, or `addFirst(x)`.
  2. In circular arrays, check if `(tail + 1) % capacity == head`; double buffer size if full.
  3. Write element at `tail` index and advance `tail = (tail + 1) % capacity`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(n) | O(n) |

Note: Worst-case O(n) time and space applies solely when dynamic circular array reaches capacity and must allocate a doubled buffer.

### Delete
- **How it works:**
  1. Often named `poll()`, `dequeue()`, `pollFirst()`, or `pollLast()`.
  2. Verify queue is not empty (`head == tail`); return null or throw exception if empty.
  3. Read element at `head`, clear memory slot (`arr[head] = null`), and advance `head = (head + 1) % capacity`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Removing the head or tail element is strictly O(1) in both array and linked list backings.

### Search
- **How it works:**
  1. Start from `head` index.
  2. Traverse circular slots forward until reaching `tail`, checking each element for equality.
  3. Return position offset if found, or -1 if absent.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Search requires O(n) linear scanning as elements are not indexed by key or value.

### Access
- **How it works:**
  1. Often named `peek()`, `element()`, `peekFirst()`, or `peekLast()`.
  2. Check if collection is empty.
  3. Return element at `head` or `tail` index without mutating pointers.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Access to boundaries (front/back) is O(1); arbitrary interior access is O(n) or unsupported.

### Update
- **How it works:**
  1. For front/rear element: overwrite `arr[head] = val` or `arr[(tail - 1 + cap) % cap] = val`.
  2. Updating arbitrary interior positions requires sequential traversal.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Updating front or rear boundaries is O(1); updating arbitrary middle elements is O(n).

### Traverse
- **How it works:**
  1. Initialize pointer at `head`.
  2. Step forward wrapping via modulo until reaching `tail`.
  3. Process each element in FIFO order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Traversal visits all n active elements in FIFO or bidirectional order.

### Sort
- **How it works:**
  1. Dequeue all elements into a temporary array or list.
  2. Sort using Dual-Pivot Quicksort or Timsort.
  3. Enqueue sorted elements back into the queue in order.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Note: Sorting directly inside a pure queue abstraction requires O(n) auxiliary memory.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Head/Tail) | O(1) | O(1) | O(n) | O(1) |
| Delete (Head/Tail) | O(1) | O(1) | O(1) | O(1) |
| Search | O(1) | O(n) | O(n) | O(1) |
| Access (Peek) | O(1) | O(1) | O(1) | O(1) |
| Update (Head/Tail) | O(1) | O(1) | O(1) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort | O(n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** Single-ended FIFO Queue (`Queue`). Trade-off: Clean FIFO semantics, but cannot efficiently push to front or pop from rear.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Deque (Double-Ended) | Allows insertion and removal at both front and rear | Operates as both Stack and Queue | Slightly more complex pointer logic | Sliding window maximum, work stealing |
| Circular Ring Buffer | Fixed capacity array with wrapping pointers | Zero heap allocations during operation | Throws or drops items when buffer is full | Audio processing, OS device drivers |
| Priority Queue | Elements dequeued by priority instead of arrival | Always extracts highest/lowest priority item | O(log n) insert/delete instead of O(1) | Dijkstra, Huffman coding, event simulators |
| BlockingQueue | Thread-safe queue that blocks on full/empty | Safe concurrent producer-consumer coordination | Thread synchronization and context switch overhead | Multithreaded thread pools, message pipelines |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Circular Array (`ArrayDeque`) | Contiguous array wrapping indices via bitmasking/modulo | Amortized O(1) operations; cache-friendly | Pre-allocated unused slots |
| Doubly Linked List (`LinkedList`) | Individual nodes with `next` and `prev` pointers | Worst-case O(1) (never resizes) | Heavy reference overhead (24-32 bytes per element) |
| Circular Linked List | Tail node's `next` pointer points back to head | Head and tail tracked via single tail pointer | Pointer chasing cache misses |

- **Java built-in equivalents:**
  - `java.util.ArrayDeque`: Circular resizable array deque; recommended default for both Queues and Deques.
  - `java.util.LinkedList`: Doubly linked node deque; implements both `Queue` and `Deque`.
  - `java.util.concurrent.ArrayBlockingQueue`: Bounded, synchronized circular array queue with reentrant locks.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyQueue.java` | Circular Array Queue & Deque | Ring-buffer queue with dynamic power-of-two resizing and Deque operations |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [05 - Stack](../05-stack/README.md) | [07 - Hashing](../07-hashing/README.md)
