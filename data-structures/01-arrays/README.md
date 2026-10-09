# Arrays
> Contiguous, fixed-size blocks of memory providing O(1) random access via arithmetic index computation.

## 1. Fundamentals
- What is it? A linear collection of elements placed in continuous memory slots where each element is reachable by an index.
- What problem does it solve? Provides fast constant-time random access and updates when collection size or bounds are known.
- What type of data does it store? Homogeneous elements (primitives such as `int`, `char`, or references to Objects).
- Linear or non-linear? Linear.
- Static or dynamic? Primitive arrays (`T[]`) are static (fixed capacity upon allocation); dynamic arrays (`ArrayList`) resize automatically.
- Ordered or unordered? Ordered by 0-based integer position index.
- Mutable or immutable? Mutable in Java: individual elements can be reassigned at any time (`arr[i] = val`), but array length is immutable.
- How is the data stored internally? Contiguously in heap memory; element address is computed as `base_address + index * element_size`.
```text
Memory layout (contiguous indices 0 to 3):
+---------+---------+---------+---------+
| arr[0]  | arr[1]  | arr[2]  | arr[3]  |
+---------+---------+---------+---------+
0x1000    0x1004    0x1008    0x100C    (for 4-byte ints)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. For dynamic arrays, verify remaining capacity; resize to larger buffer (typically 1.5x) if full.
  2. Shift existing elements from target index through end one position right.
  3. Place new element into the vacant slot and increment size.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(n) |

Note: Appending to the end without resizing is O(1); inserting at index 0 requires shifting all n elements (O(n)).

### Delete
- **How it works:**
  1. Locate target element or index.
  2. Shift all subsequent elements from index + 1 down by one position to overwrite target.
  3. Clear last slot (null reference in object arrays) and decrement size.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Deleting the last element is O(1); deleting from index 0 shifts n - 1 elements (O(n)).

### Search
- **How it works:**
  1. For linear search, inspect each index from 0 to n - 1 until a match is found.
  2. For sorted arrays, apply binary search by comparing query against midpoint and halving the search interval.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Search by value requires O(n) on unsorted arrays, or O(log n) if previously sorted.

### Access
- **How it works:**
  1. Validate index is within bounds [0, size - 1].
  2. Compute memory address: `base + index * element_size`.
  3. Read value directly from computed address in a single instruction.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Access by index is unconditional O(1) due to direct pointer arithmetic.

### Update
- **How it works:**
  1. Validate index is within bounds [0, size - 1].
  2. Compute target memory address directly.
  3. Overwrite value in memory slot.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Update at a known index is always O(1).

### Traverse
- **How it works:**
  1. Initialize pointer or index variable at 0.
  2. Visit each element sequentially up to length - 1.
  3. Terminate when upper boundary is reached.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Sequential iteration visits all n elements exactly once with optimal CPU cache spatial locality.

### Sort
- **How it works:**
  1. Reorder elements in ascending/descending order.
  2. Java uses Dual-Pivot Quicksort for primitives and Timsort for Object arrays.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n log n) | O(log n) |
| Worst | O(n log n) | O(n) |

Note: In-place Dual-Pivot Quicksort uses O(log n) stack space; Timsort requires O(n) auxiliary memory for runs.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert | O(1) | O(n) | O(n) | O(1) |
| Delete | O(1) | O(n) | O(n) | O(1) |
| Search (Unsorted) | O(1) | O(n) | O(n) | O(1) |
| Access | O(1) | O(1) | O(1) | O(1) |
| Update | O(1) | O(1) | O(1) | O(1) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort | O(n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** Fixed-size static array (`T[]`). Trade-off: Maximum cache locality and zero memory overhead per element, but capacity cannot change once allocated.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Dynamic Array (`ArrayList`) | Automatically reallocates larger memory buffer when full | Flexible size; amortized O(1) append | Resizing causes O(n) latency spikes; memory overhead | Unknown or growing collection size |
| Circular / Ring Buffer | Head and tail wrap around using modulo arithmetic | O(1) insertion and deletion at both ends | Fixed capacity; indexing requires modulo | Queues, producer-consumer stream buffering |
| BitSet | Packed binary bits inside long words (`long[]`) | 8x to 64x memory reduction for boolean flags | Bitwise arithmetic overhead | Dense set representation, sieve algorithms |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Static Array (`int[]`) | Fixed size allocated contiguously on heap | Predictable O(1) access/update; no resize penalty | Zero capacity overhead beyond array header |
| Geometric Resizing (`ArrayList`) | Doubles or expands capacity by 1.5x on overflow | Amortized O(1) append; occasional O(n) copy | Up to 33%-50% unused buffer capacity |
| Incremental Resizing | Expands buffer by fixed constant k each time | Avoids large over-allocations | Amortized O(n) append due to frequent reallocations |

- **Java built-in equivalents:**
  - `T[]` (primitive/object array): Raw contiguous JVM array object with fixed `.length` field.
  - `java.util.ArrayList`: Backed by an internal `Object[] elementData` array; grows by 50% (`newCapacity = oldCapacity + (oldCapacity >> 1)`).
  - `java.util.Vector`: Synchronized dynamic array (thread-safe, legacy, doubling capacity strategy).

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyArrayList.java` | Dynamic resizable array | Custom generic resizable array with geometric growth |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | Start | [02 - Strings](../02-strings/README.md)
