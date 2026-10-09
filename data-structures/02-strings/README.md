# Strings
> Ordered sequence of characters backed by contiguous byte or character arrays, immutable in standard Java.

## 1. Fundamentals
- What is it? A linear collection of characters used to represent textual information.
- What problem does it solve? Enables structured storage, formatting, transmission, and pattern manipulation of human-readable text.
- What type of data does it store? Character data (`char` 16-bit code units or compact 8-bit ISO-8859-1/Latin-1 bytes).
- Linear or non-linear? Linear.
- Static or dynamic? `String` is static in length; `StringBuilder` and `StringBuffer` provide dynamic resizing.
- Ordered or unordered? Strictly ordered by sequence index from 0 to length - 1.
- Mutable or immutable? `java.lang.String` is strictly immutable in Java (thread-safe, shareable via string pool); `StringBuilder` is mutable.
- How is the data stored internally? In Java 9+, stored as `byte[] value` with a `byte coder` flag (0 for Latin-1, 1 for UTF-16) to conserve memory.
```text
Memory layout (Java 9+ compact string):
+---------+  value  +-----+-----+-----+-----+
| coder:0 | ------> | 'H' | 'e' | 'l' | 'l' |
+---------+         +-----+-----+-----+-----+
                    0x200 0x201 0x202 0x203
```

## 2. Core Operations

### Insert
- **How it works:**
  1. For immutable `String`, allocate a new char/byte array of size `n + k`.
  2. Copy prefix [0, index), copy inserted characters, and copy suffix [index, n).
  3. Instantiate a new `String` object wrapping the newly allocated array.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Inserting into immutable `String` always copies full content (O(n)). `StringBuilder.append()` is amortized O(1).

### Delete
- **How it works:**
  1. Allocate new backing array of size `n - k`.
  2. Copy characters before the target substring and characters following the target substring.
  3. Create and return a new `String` reference.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Deletion in immutable `String` produces a new instance of size O(n). In `StringBuilder`, shifting takes O(n) time and O(1) space.

### Search
- **How it works:**
  1. Scan text characters comparing against target character or pattern substring.
  2. Standard `indexOf` performs naive sliding comparison.
  3. Advanced search algorithms (KMP, Boyer-Moore, Rabin-Karp) use preprocessed shift tables or polynomial rolling hashes.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n * m) | O(1) |

Note: Single character lookup is O(n); substring match of pattern length m in text of length n is O(n * m) naive, or O(n + m) via KMP.

### Access
- **How it works:**
  1. Verify index lies within [0, length - 1].
  2. For compact strings, read byte at index (or decode pair if UTF-16).
  3. Return character value.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Access by index via `.charAt(i)` is constant time O(1).

### Update
- **How it works:**
  1. Because Java `String` is immutable, in-place updates are not supported.
  2. Construct a new `char[]` or use `StringBuilder.setCharAt()`.
  3. Copy modified content into a newly allocated string instance.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n) | O(n) |
| Worst | O(n) | O(n) |

Note: Modifying a single character in immutable `String` requires full string duplication O(n). In `StringBuilder`, it is O(1) in-place.

### Traverse
- **How it works:**
  1. Iterate index from 0 to `length() - 1`.
  2. Access each character via `charAt(i)` or unpack `toCharArray()`.
  3. Process character sequentially.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(1) |
| Average | O(n) | O(1) |
| Worst | O(n) | O(1) |

Note: Standard loop traversal visits every character in O(n) time without extra memory.

### Sort
- **How it works:**
  1. Extract characters into a mutable primitive array `char[] chars = str.toCharArray()`.
  2. Sort array using Dual-Pivot Quicksort or counting sort for bounded alphabets (e.g. ASCII 256).
  3. Instantiate a new `String` from the sorted array.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(n) | O(n) |
| Average | O(n log n) | O(n) |
| Worst | O(n log n) | O(n) |

Note: Counting sort achieves O(n + alphabet) time and O(alphabet) space; general sort is O(n log n).

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (String) | O(n) | O(n) | O(n) | O(n) |
| Delete (String) | O(n) | O(n) | O(n) | O(n) |
| Search (Substring) | O(1) | O(n) | O(n * m) | O(1) |
| Access | O(1) | O(1) | O(1) | O(1) |
| Update (String) | O(n) | O(n) | O(n) | O(n) |
| Traverse | O(n) | O(n) | O(n) | O(1) |
| Sort | O(n) | O(n log n) | O(n log n) | O(n) |

## 3. Variations
- **Standard version:** `java.lang.String` (immutable sequence). Trade-off: Thread-safety, hashing caching, and string interning, but frequent modifications trigger heavy garbage collection overhead.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| `StringBuilder` | Mutable resizable character buffer | In-place modifications; amortized O(1) appends | Not thread-safe | Sequential string construction, parsing |
| `StringBuffer` | Synchronized mutable character buffer | Thread-safe in-place mutation | Synchronization locking overhead | Multi-threaded string concatenation |
| Rope | Binary tree where leaves contain string slices | O(log n) concatenation and splitting of massive text | Complex tree balance; high pointer overhead | Text editors, large document processing |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Immutable String | Shared internal buffer, copy on write | Read-only O(1) shares; O(n) write copy | Deduplication via string pool; memory waste on churn |
| Mutable Dynamic Buffer | Over-allocated array with capacity doubling | O(1) amortized appends; fast in-place mutations | Temporary excess capacity buffer |
| Tree / Rope | Tree structure composed of linked segments | O(log n) split and concatenate; O(log n) access | Substantial tree node reference overhead |

- **Java built-in equivalents:**
  - `java.lang.String`: Immutable character sequence with intern cache and cached hash code (`private int hash`).
  - `java.lang.StringBuilder`: Mutable dynamic character sequence backed by `byte[]` / `char[]` buffer with 2x + 2 growth policy.
  - `java.lang.StringBuffer`: Synchronized version of `StringBuilder` using method-level locks.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyStringBuilder.java` | Mutable resizable string buffer | Custom resizable char array buffer with append, insert, delete |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [01 - Arrays](../01-arrays/README.md) | [03 - Matrix & 2D Arrays](../03-matrix-2d-arrays/README.md)
