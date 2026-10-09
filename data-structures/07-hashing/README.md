# Hashing
> Associative data structure mapping keys to values via hash functions for average O(1) operations.

## 1. Fundamentals
- What is it? An associative dictionary mapping unique keys to values by running keys through a hash function to produce bucket array indices.
- What problem does it solve? Provides constant-time O(1) average lookups, insertions, and removals for arbitrary keys without sequential scanning.
- What type of data does it store? Key-value pairs (`Map<K, V>`) or distinct single keys (`Set<K>`).
- Linear or non-linear? Non-linear (associative mapping structure).
- Static or dynamic? Dynamic: expands bucket capacity and rehashes entries when current element count exceeds `capacity * loadFactor` (default 0.75 in Java).
- Ordered or unordered? Unordered by default in `HashMap`; `LinkedHashMap` maintains insertion/access order; `TreeMap` enforces sorted order.
- Mutable or immutable? Mutable in Java: entries can be added, updated, or removed dynamically.
- How is the data stored internally? An array of bucket pointers (`Node<K, V>[] table`). Collisions are resolved via separate chaining (singly linked list, converted to a Red-Black tree in Java 8+ if a bucket holds 8 or more entries).
```text
HashMap Internal Memory Layout (Java 8+):
table (array)
+-------+
|  [0]  | ----> [ K1,V1 | next ] ---> [ K2,V2 | null ]  (Singly Linked List chain)
+-------+
|  [1]  | ----> null
+-------+
|  [2]  | ----> [ TreeNode: Root ]                      (Red-Black Tree when >= 8 nodes)
|       |         /           \
|       |     [ TreeNode ]  [ TreeNode ]
+-------+
|  [3]  | ----> [ K3,V3 | null ]
+-------+
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `put(key, value)`. Compute key hash: `h = key.hashCode() ^ (h >>> 16)`.
  2. Map hash to bucket index: `index = (n - 1) & h` (where `n` is power-of-two table capacity).
  3. Traverse chain/tree at `table[index]`. If key already exists, overwrite value; otherwise append new node and rehash if load factor threshold is breached.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(n) |

Note: Worst-case is O(log n) in Java 8+ due to bucket treeification (O(n) in naive separate chaining without treeification). Worst-case space is O(n) during table resize.

### Delete
- **How it works:**
  1. Often named `remove(key)`. Compute key hash code and target bucket index.
  2. Search bucket chain or Red-Black tree for matching key via `key.equals()`.
  3. Unlink node from chain (or execute Red-Black tree deletion) and decrement size.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: Worst-case is O(log n) with treeified bins; O(1) average under uniform hash distribution.

### Search
- **How it works:**
  1. Often named `containsKey(key)`. Compute bucket index from key hash.
  2. Walk bucket linked list or balanced tree matching `entry.hash == h && (entry.key == key || key.equals(entry.key))`.
  3. Return true if matched, false if end of bucket is reached.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: Average search is constant O(1); treeified collision bins cap worst case at O(log n).

### Access
- **How it works:**
  1. Often named `get(key)`. Compute hash and locate bucket index.
  2. Locate matching node within bucket chain or tree.
  3. Return `node.value` if found, or `null` if absent.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: Access by key matches search complexity; direct positional access by integer index is not supported.

### Update
- **How it works:**
  1. Compute hash and locate bucket index.
  2. Locate existing entry with matching key.
  3. Overwrite `node.value = newValue` and return old value.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(log n) | O(1) |

Note: Updating value of an existing key is O(1) average.

### Traverse
- **How it works:**
  1. Iterate across all bucket array indices from 0 to `capacity - 1`.
  2. For every non-null bucket, traverse all linked/tree nodes in that chain.
  3. Yield each key, value, or `Map.Entry<K, V>`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n + capacity) | O(1) |
| Average | O(n + capacity) | O(1) |
| Worst | O(n + capacity) | O(1) |

Note: Traversal visits all entries and checks all empty bucket slots, requiring O(n + capacity) time.

### Sort
- **How it works:**
  1. Hash tables do not maintain sorted order natively.
  2. To sort, extract entries into an `ArrayList<Map.Entry<K, V>>` and apply Timsort, or insert all pairs into a `TreeMap`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n log n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Note: Direct in-place sorting is N/A because hashing destroys relative key ordering.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Put) | O(1) | O(1) | O(log n) | O(1) |
| Delete (Remove) | O(1) | O(1) | O(log n) | O(1) |
| Search (containsKey) | O(1) | O(1) | O(log n) | O(1) |
| Access (Get) | O(1) | O(1) | O(log n) | O(1) |
| Update | O(1) | O(1) | O(log n) | O(1) |
| Traverse | O(n + cap) | O(n + cap) | O(n + cap) | O(1) |
| Sort (via TreeMap) | O(n log n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** Separate chaining hash table (`HashMap`). Trade-off: Robust against high load factors without catastrophic probe degradation, but node allocations introduce memory and pointer overhead.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Separate Chaining | Colliding elements linked in bucket chains or trees | Simple deletion; handles load factor > 1 | Node object allocation overhead | General purpose dictionaries (`HashMap`) |
| Open Addressing (Linear Probing) | Colliding keys placed in next available slot in table | Superior cache locality; no node objects | Primary clustering; degrades above load factor 0.7 | High-performance primitive maps, caching |
| Robin Hood Hashing | Displaces entries with shorter probe distances on insertion | Low lookup variance; fast failed searches | Complex insertion logic with frequent swaps | Read-heavy hash tables, compiler symbol tables |
| Cuckoo Hashing | Uses 2+ hash functions and alternative tables | Strict O(1) worst-case lookup guarantee | Insertions can trigger infinite loops (requires rehashing) | Hardware lookups, networking routers |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Separate Chaining (Java 8+) | Array of node pointers; chains treeify at threshold 8 | O(1) average; worst-case bounded to O(log n) | Node object header and reference overhead |
| Linear Probing | Flat array; probes `(index + i) % cap` on collision | O(1) average; worst-case O(n) under clustering | Single flat array, zero pointer overhead |
| Double Hashing | Probes `(hash1(k) + i * hash2(k)) % cap` | Eliminates primary and secondary clustering | Requires computing second independent hash |

- **Java built-in equivalents:**
  - `java.util.HashMap`: General separate chaining hash table with treeification.
  - `java.util.HashSet`: Set backed internally by a `HashMap` where values are dummy Object instances.
  - `java.util.LinkedHashMap`: Subclass of `HashMap` maintaining a doubly linked list through entries to preserve insertion or LRU access order.
  - `java.util.concurrent.ConcurrentHashMap`: Thread-safe, lock-striped hash table using CAS and synchronized bucket heads.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyHashMap.java` | Separate Chaining Hash Map | Custom hash map with power-of-two capacity, entry chains, and dynamic rehashing |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [06 - Queue & Deque](../06-queue-deque/README.md) | [08 - Binary Tree](../08-binary-tree/README.md)
