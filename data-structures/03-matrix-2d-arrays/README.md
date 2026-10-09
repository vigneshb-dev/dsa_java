# Matrix & 2D Arrays
> Two-dimensional grid data structures accessed via row and column coordinates.

## 1. Fundamentals
- What is it? A two-dimensional collection of elements structured into rows and columns accessed by coordinate pair `(r, c)`.
- What problem does it solve? Represents grids, spatial boards (chess, maps), image pixels, geometric coordinate planes, and dynamic programming state tables.
- What type of data does it store? Homogeneous primitive data types or Object references.
- Linear or non-linear? Non-linear (two-dimensional grid abstraction composed of nested linear arrays).
- Static or dynamic? Static in dimensions for primitive arrays `T[R][C]`; dynamic when implemented using nested lists `List<List<T>>`.
- Ordered or unordered? Ordered by row-index and column-index coordinates.
- Mutable or immutable? Mutable in Java: cells can be modified in-place (`matrix[r][c] = val`).
- How is the data stored internally? In Java, stored as an "array of arrays" where an outer array contains references pointing to distinct 1D row array objects on the heap.
```text
Java Array-of-Arrays Memory Layout:
matrix ----> [ ptr0 | ptr1 | ptr2 ]   (Outer array of row references)
                |      |      |
                v      v      v
row 0:        [0,0]  [0,1]  [0,2]     (Individual contiguous 1D array)
row 1:        [1,0]  [1,1]  [1,2]     (Individual contiguous 1D array)
row 2:        [2,0]  [2,1]  [2,2]     (Individual contiguous 1D array)
```

## 2. Core Operations

### Insert
- **How it works:**
  1. To insert a row in a dynamic 2D structure, allocate and append a new 1D array reference.
  2. To insert a column into an existing fixed matrix, reallocate all rows with width + 1 and shift elements rightward.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(C) |
| Average | O(R * C) | O(R * C) |
| Worst | O(R * C) | O(R * C) |

Note: Appending a row to a list of rows is O(1) amortized; inserting a column into all rows requires shifting every row (O(R * C)).

### Delete
- **How it works:**
  1. To delete a row, remove row reference and shift remaining row references up.
  2. To delete a column, iterate through all rows, shift elements from col + 1 leftward by 1, and shrink row buffers.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(R * C) | O(1) |
| Worst | O(R * C) | O(1) |

Note: Removing a row is O(R) reference shifts; removing a column requires shifting elements across every row (O(R * C)).

### Search
- **How it works:**
  1. In an unsorted matrix, scan every cell sequentially across all rows and columns.
  2. In a row-and-column sorted matrix (Young Tableau), start at top-right corner; step left if target is smaller, step down if target is larger.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(R * C) | O(1) |
| Worst | O(R * C) | O(1) |

Note: Linear scan takes O(R * C); staircase search on sorted matrices runs in O(R + C).

### Access
- **How it works:**
  1. Dereference row pointer in the outer array: `matrix[r]`.
  2. Index into the target column inside that row: `[c]`.
  3. Retrieve value directly.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Cell access is O(1) via two consecutive pointer dereferences.

### Update
- **How it works:**
  1. Validate bounds: `0 <= r < R` and `0 <= c < C`.
  2. Dereference row reference and update target slot: `matrix[r][c] = val`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Updating a cell by known coordinate pair is always O(1).

### Traverse
- **How it works:**
  1. Row-major traversal: outer loop iterates rows `r`, inner loop iterates columns `c`.
  2. Column-major traversal: outer loop iterates columns `c`, inner loop iterates rows `r`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(R * C) | O(1) |
| Average | O(R * C) | O(1) |
| Worst | O(R * C) | O(1) |

Note: Row-major traversal maximizes CPU cache hit rate because row elements reside in contiguous memory chunks.

### Sort
- **How it works:**
  1. Either sort each row independently in O(R * C log C).
  2. Or flatten all R * C elements into a 1D array, sort globally in O((RC) log(RC)), and repopulate matrix.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(R * C) | O(R * C) |
| Average | O(R * C log(R * C)) | O(R * C) |
| Worst | O(R * C log(R * C)) | O(R * C) |

Note: Global sorting requires O(R * C) temporary buffer space.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (Row) | O(1) | O(R) | O(R) | O(C) |
| Insert (Column) | O(R) | O(R * C) | O(R * C) | O(R * C) |
| Delete (Row) | O(1) | O(R) | O(R) | O(1) |
| Delete (Column) | O(R) | O(R * C) | O(R * C) | O(1) |
| Search (Unsorted) | O(1) | O(R * C) | O(R * C) | O(1) |
| Access | O(1) | O(1) | O(1) | O(1) |
| Update | O(1) | O(1) | O(1) | O(1) |
| Traverse | O(R * C) | O(R * C) | O(R * C) | O(1) |
| Sort (Global) | O(R * C) | O(R * C log(R * C)) | O(R * C log(R * C)) | O(R * C) |

## 3. Variations
- **Standard version:** Java 2D array (`T[][]`, array of row references). Trade-off: Simplicity and support for jagged lengths, but double pointer dereference and scattered row allocations reduce cache efficiency compared to flat buffers.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Flat 1D Array (`int[R * C]`) | Maps 2D coordinate `(r, c)` to `r * C + c` in a single buffer | Perfect spatial cache locality; single heap object | Requires manual index arithmetic | High-performance numerical computing, image buffers |
| Jagged / Ragged Array | Each row array has a different length (`arr[i].length != arr[j].length`) | Saves memory when row lengths vary naturally | Non-uniform bounds checking required | Triangular tables, adjacency lists, grouping |
| Sparse Matrix (CSR / COO) | Stores only non-zero entries using value and index arrays | Massive memory savings when most cells are zero | O(k) or O(log k) cell lookup overhead | Graph Laplacians, large sparse graphs, NLP embeddings |
| Dynamic 2D List (`List<List<T>>`) | Nested `ArrayList` objects | Dynamic row and column additions | Substantial object boxing and pointer overhead | Dynamically growing grids and boards |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Array-of-Arrays (`int[][]`) | Outer pointer array pointing to distinct row arrays | Double memory dereference for access | Header overhead for outer array plus R inner row arrays |
| Flat 1D Array (`int[]`) | Contiguous single array indexed via `r * C + c` | Single dereference; maximum hardware prefetch | Zero overhead beyond one array object header |
| Coordinate List (COO) | Lists of tuples `(row, col, value)` | O(non-zeros) iteration; slower random access | O(3 * non-zeros) memory; compact if non-zeros << R * C |

- **Java built-in equivalents:**
  - `T[][]`: Native Java array-of-arrays representation.
  - `java.util.ArrayList<ArrayList<T>>`: Dynamic nested list structure.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyMatrix.java` | Flat 1D-backed 2D Matrix | Contiguous 1D backing buffer with 2D coordinate mapping and matrix operations |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [02 - Strings](../02-strings/README.md) | [04 - Linked List](../04-linked-list/README.md)
