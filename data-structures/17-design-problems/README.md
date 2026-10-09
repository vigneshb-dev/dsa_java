# Design Problems
> Composite data structure architectures combining hashing, linked lists, and heaps to satisfy multi-faceted O(1) or O(log n) performance contracts.

## 1. Fundamentals
- What is it? Composite data structures engineered by coupling multiple algorithmic primitives (e.g., hash tables, doubly linked lists, dual heaps) to satisfy simultaneous performance requirements.
- What problem does it solve? Solves specialized operational contracts such as cache replacement policies (LRU, LFU), constant-time auxiliary queries (Min Stack), and dynamic stream percentiles (Median Finder).
- What type of data does it store? Composite records containing primary keys, values, timestamps, frequency counters, and bidirectional linkage pointers.
- Linear or non-linear? Hybrid: combinations of linear linked chains and associative hash tables or hierarchical heaps.
- Static or dynamic? Dynamic: accommodates continuous streaming inputs, real-time mutations, and bounded capacity evictions.
- Ordered or unordered? Multi-ordered: maintains fast associative key lookups while simultaneously preserving temporal access recency, frequency ranks, or sorted numerical order.
- Mutable or immutable? Mutable in Java: node positions, frequencies, and references mutate dynamically on read and write operations.
- How is the data stored internally? Hybrid memory architecture: typically combines a hash table for $O(1)$ key-to-node dereferencing with an intrusive doubly linked list or heaps for $O(1)$ priority rearrangement.
```text
LRU Cache Composite Architecture:
HashMap: { Key -> NodePtr }
               |
               v
DLL: [Head / Most Recent] <==> [Node A] <==> [Node B] <==> [Tail / Least Recent]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. LRU Cache `put(k, v)`: Look up key in map; if exists, update value and move node to head in $O(1)$. If absent, check capacity; evict tail if full, allocate new node, insert at head, and add to map in $O(1)$.
  2. Min Stack `push(x)`: Push value to main stack; push $\min(x, \text{currentMin})$ to min tracking stack in $O(1)$.
  3. Median Finder `addNum(x)`: Insert into Max-Heap or Min-Heap, balance heap sizes to maintain size difference $\le 1$ in $O(\log n)$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: LRU/LFU/MinStack insertions are $O(1)$ amortized; MedianFinder heap insertion is $O(\log n)$.

### Delete
- **How it works:**
  1. LRU Cache Eviction: Unlink tail node from doubly linked list and remove corresponding key from hash map in $O(1)$.
  2. Min Stack `pop()`: Pop top element from primary stack and min stack simultaneously in $O(1)$.
  3. LFU Cache Eviction: Identify minimum frequency list, remove its tail node, and remove key from hash map in $O(1)$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Eviction and popping operations operate in strict constant $O(1)$ time.

### Search
- **How it works:**
  1. In LRU/LFU Caches, inspect `map.containsKey(key)` to determine presence in $O(1)$ average time.
  2. In RandomizedSet, check `map.containsKey(val)` in $O(1)$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: Search is supported in constant time through hash map indexing.

### Access
- **How it works:**
  1. LRU Cache `get(key)`: Locate node in map in $O(1)$; splice node out of current position and insert at head (most recently used); return value.
  2. Min Stack `getMin()`: Peek top of auxiliary min stack in $O(1)$.
  3. Median Finder `findMedian()`: Peek roots of max-heap and min-heap in $O(1)$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: All composite design structures target strict $O(1)$ access for their specialized query target.

### Update
- **How it works:**
  1. LRU Cache: Mutating existing key value reorders node to the head of the recency list in $O(1)$.
  2. LFU Cache: Increment node frequency count, move node from old frequency list to `freq + 1` list in $O(1)$.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Relinking pointers inside doubly linked lists executes in $O(1)$ time.

### Traverse
- **How it works:**
  1. Walk the doubly linked list sequentially from head to tail to inspect items in order of recency or frequency.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Sequential iteration visits all active cache entries in $O(n)$ time.

### Sort
- **How it works:**
  1. Relative priority (recency or frequency) is maintained dynamically on every operation.
  2. N/A for raw re-sorting; structures maintain targeted partial or total order invariants continuously.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | N/A | N/A |
| Average | N/A | N/A |
| Worst | N/A | N/A |

Note: Explicit sorting is N/A because ordering is an incremental byproduct of operations.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| LRU Get / Put | O(1) | O(1) | O(1) | O(1) |
| LFU Get / Put | O(1) | O(1) | O(1) | O(1) |
| MinStack Push / Pop / Min | O(1) | O(1) | O(1) | O(1) |
| Median AddNum | O(log n) | O(log n) | O(log n) | O(1) |
| Median FindMedian | O(1) | O(1) | O(1) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort | N/A | N/A | N/A | N/A |

## 3. Variations
- **Standard version:** LRU Cache (HashMap + Doubly Linked List). Trade-off: Strict $O(1)$ time for both get and put operations, but requires pointer manipulations and node object allocations.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| LRU Cache | Evicts least recently accessed element | $O(1)$ get and put; adapts quickly to temporal access shifts | Vulnerable to scan pollution | Web caches, database buffer pools |
| LFU Cache | Evicts least frequently used element; breaks ties via recency | Keeps historically popular items; immune to one-off scans | Stale hot items can stay indefinitely; complex dual map | Content Delivery Networks (CDNs) |
| Min / Max Stack | Augments stack with running min tracking | $O(1)$ push, pop, and getMin | 2x memory to store auxiliary minimums | Sliding window minimums, math evaluators |
| Median Finder | Uses balanced Max-Heap (lower half) and Min-Heap (upper half) | $O(1)$ median retrieval; dynamic streaming support | $O(\log n)$ insertion cost | Real-time sensor percentiles, financial feeds |
| RandomizedSet | Combines `ArrayList` and `HashMap<Value, Index>` | $O(1)$ insert, delete, and `getRandom()` | Value must be hashable; array shifts on deletion | Randomized sampling, load balancing |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| HashMap + Doubly Linked List | Map stores node references; list maintains MRU to LRU order | Strict $O(1)$ node insertion, removal, and lookup | Node objects with `prev`, `next`, `key`, `val` references |
| Dual Heaps | Max-heap for lower 50% and min-heap for upper 50% of stream | $O(1)$ median access; $O(\log n)$ number addition | Two contiguous array buffers |
| Array + Hash Map | Elements in dynamic array; map records index of each element | $O(1)$ swap-with-tail deletion and uniform random index | Array buffer plus hash map entries |

- **Java built-in equivalents:**
  - `java.util.LinkedHashMap`: Can function as an LRU Cache by overriding `protected boolean removeEldestEntry(Map.Entry<K, V> eldest)`:
    ```java
    new LinkedHashMap<K, V>(capacity, 0.75f, true) {
        @Override
        protected boolean removeEldestEntry(Map.Entry<K, V> eldest) {
            return size() > capacity;
        }
    };
    ```

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyLRUCache.java` | LRU Cache | Custom HashMap and Doubly Linked List implementation supporting O(1) get and put |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [16 - Fenwick Tree](../16-fenwick-tree/README.md) | End
