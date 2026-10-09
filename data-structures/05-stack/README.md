# Stack
> Last-In, First-Out (LIFO) linear data structure where all insertions and deletions occur strictly at the top.

## 1. Fundamentals
- What is it? A constrained linear collection operating under the Last-In, First-Out (LIFO) protocol.
- What problem does it solve? Manages nested state hierarchies, execution call stacks, parenthesis matching, undo history, and depth-first search graph traversal.
- What type of data does it store? Generic elements (primitives or object references).
- Linear or non-linear? Linear.
- Static or dynamic? Static when implemented via fixed-size array; dynamic when implemented via resizable array or linked list.
- Ordered or unordered? Ordered strictly by arrival sequence (temporal ordering).
- Mutable or immutable? Mutable in Java: states change dynamically with push and pop operations.
- How is the data stored internally? Backed internally either by a contiguous array with a `top` integer pointer, or by a singly linked list where top points to head node.
```text
Stack (LIFO) Internal Layout:
         +-------+
top ---> | item3 |  <--- Push / Pop operations
         +-------+
         | item2 |
         +-------+
         | item1 |
         +-------+
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `push(x)`. Check if array capacity is exceeded and expand buffer if necessary.
  2. Write element to index `top + 1` (or prepend node to head in linked implementation).
  3. Increment `top` pointer and return inserted value.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(n) | O(n) |

Note: Worst-case O(n) occurs only during array resizing in dynamic-array backed implementations; linked implementations are always O(1).

### Delete
- **How it works:**
  1. Often named `pop()`. Check if stack is empty (`top == -1`); throw `EmptyStackException` or return null.
  2. Read element at `top`.
  3. Clear reference at `top` (to avoid memory leaks), decrement `top`, and return the popped value.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Removing the top element is always O(1).

### Search
- **How it works:**
  1. Search for value x by scanning from top down to bottom index 0.
  2. In `java.util.Stack.search(o)`, returns 1-based distance from top if found, or -1 if absent.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Best case occurs when the sought item is at the top of the stack.

### Access
- **How it works:**
  1. Often named `peek()`. Verify stack is non-empty.
  2. Read and return the element at position `top` without modifying the pointer or removing the element.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Only the top element can be accessed in O(1); arbitrary index access violates the LIFO stack abstraction.

### Update
- **How it works:**
  1. For top element: overwrite `array[top] = newVal`.
  2. For arbitrary interior elements: requires popping off overlying elements or violating the stack abstraction via direct backing array access.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Updating the top element is O(1); updating deep elements is not supported in standard pure stack interfaces.

### Traverse
- **How it works:**
  1. Iterate sequentially from index `top` down to 0 (or following `next` pointers from top to tail).
  2. Inspect each element without modifying stack state.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Iteration examines all n elements in LIFO order.

### Sort
- **How it works:**
  1. Using an auxiliary stack: pop top element from original stack, hold in temp.
  2. While auxiliary stack is not empty and auxiliary top > temp, pop auxiliary back to original.
  3. Push temp onto auxiliary stack; repeat until original is empty, then transfer back.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n^2) | O(n) |
| Worst | O(n^2) | O(n) |

Note: Pure stack sorting using an auxiliary stack takes O(n^2) time; copying to an array and sorting takes O(n log n).

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Push) | O(1) | O(1) | O(n) | O(1) |
| Delete (Pop) | O(1) | O(1) | O(1) | O(1) |
| Search | O(1) | O(n) | O(n) | O(1) |
| Access (Peek) | O(1) | O(1) | O(1) | O(1) |
| Update (Top) | O(1) | O(1) | O(1) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort (Aux Stack) | O(n) | O(n^2) | O(n^2) | O(n) |

## 3. Variations
- **Standard version:** Array-backed LIFO stack (`ArrayDeque`). Trade-off: Constant O(1) push/pop with optimal cache locality, but occasional array resizing overhead and lack of indexed access.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Min / Max Stack | Stores value alongside current running minimum/maximum | O(1) minimum retrieval alongside push/pop | 2x memory usage to store auxiliary minimums | Sliding windows, stock span problems |
| Monotonic Stack | Enforces strictly increasing or decreasing element values | Discovers Next Greater / Smaller Element in O(n) total | Drops non-conforming elements on push | Histogram area, temperature spans |
| Two Stacks in One Array | Two stacks grow inward from index 0 and index n - 1 | Zero wasted array capacity between two stacks | Hard capacity limit (array length) | Dual buffer evaluation, memory-constrained devices |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Array-Backed (`ArrayDeque`) | Circular/linear array with resizing on overflow | Fastest push/pop due to memory locality; amortized O(1) | Unused allocated capacity buffer |
| Node-Backed (`LinkedList`) | Singly linked nodes pushed/popped at head | Strict O(1) worst-case per operation; no resize spike | 24-32 bytes overhead per node object |
| Vector-Backed (`Stack`) | Legacy subclass of `Vector` with method-level synchronization | Thread-safe | High synchronization lock latency penalty |

- **Java built-in equivalents:**
  - `java.util.ArrayDeque`: Recommended modern stack implementation; resizable array-backed, unsynchronized, faster than `Stack` and `LinkedList`.
  - `java.util.LinkedList`: Implements `Deque`; node-backed alternative.
  - `java.util.Stack`: Legacy synchronized class extending `Vector`; discouraged by official Java documentation.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyStack.java` | Resizable Array Stack & Min Stack | Array-backed LIFO stack with dynamic growth and O(1) getMin support |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [04 - Linked List](../04-linked-list/README.md) | [06 - Queue & Deque](../06-queue-deque/README.md)
