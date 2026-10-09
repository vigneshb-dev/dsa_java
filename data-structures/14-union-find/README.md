# Union-Find
> Disjoint-set data structure maintaining partitioned equivalence classes with near-constant time connectivity and union operations.

## 1. Fundamentals
- What is it? A data structure that tracks elements partitioned into disjoint (non-overlapping) subsets, supporting finding an element's set and uniting two sets.
- What problem does it solve? Answers dynamic connectivity queries, cycle detection in undirected graphs, percolation thresholds, and edge selection in Kruskal's MST in near-constant time.
- What type of data does it store? Disjoint integer element identifiers representing set membership.
- Linear or non-linear? Non-linear (forest of shallow directed trees pointing from child to parent representative).
- Static or dynamic? Dynamic connectivity; typically fixed in element universe size $N$.
- Ordered or unordered? Unordered: models equivalence classes without inherent ordering.
- Mutable or immutable? Mutable in Java: internal parent pointers and rank/size arrays are updated during union and find operations.
- How is the data stored internally? Packed in flat integer arrays: `int[] parent` (where `parent[i] == i` indicates a root representative) and `int[] rank` (or `size[]`).
```text
Union-Find Forest Layout:
Set 0 Representative: 0           Set 3 Representative: 3
       /        \                          |
     (1)        (2)                       (4)

parent[]: [ 0, 0, 0, 3, 3 ]
rank[]:   [ 1, 0, 0, 1, 0 ]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Often named `makeSet(x)`. Initialize a new isolated element.
  2. Assign `parent[x] = x`, and `rank[x] = 0` (or `size[x] = 1`).
  3. Increments total component count by 1.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(1) | O(1) |

Note: Adding a new isolated element is strictly constant O(1).

### Delete
- **How it works:**
  1. Standard Union-Find cannot delete or split sets once merged.
  2. N/A: Undoing merges is not supported in classical DSU (requires DSU with Rollback using an explicit operation history stack).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | N/A | N/A |
| Average | N/A | N/A |
| Worst | N/A | N/A |

Note: Deleting an edge or splitting a component is unsupported in standard DSU because path compression destroys tree topology.

### Search
- **How it works:**
  1. Often named `find(i)`. Trace parent pointers from $i$ up to root where `parent[root] == root`.
  2. Apply Path Compression: update all visited nodes along the search path to point directly to the root (`parent[i] = find(parent[i])`).
  3. Return root representative.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(\alpha(N)) | O(1) |
| Worst | O(\alpha(N)) | O(1) |

Note: $\alpha(N)$ is the Inverse Ackermann function ($\alpha(N) < 5$ for all realistic $N$ in the physical universe).

### Access
- **How it works:**
  1. Often named `connected(u, v)`.
  2. Compute root representative of $u$: `rootU = find(u)`.
  3. Compute root representative of $v$: `rootV = find(v)`. Return `rootU == rootV`.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(\alpha(N)) | O(1) |
| Worst | O(\alpha(N)) | O(1) |

Note: Verifying if two elements share the same connected component takes two `find` calls ($O(\alpha(N))$).

### Update
- **How it works:**
  1. Often named `union(u, v)`. Find roots: `rootU = find(u)` and `rootV = find(v)`.
  2. If `rootU == rootV`, elements are already connected; return false.
  3. Union by Rank/Size: attach smaller tree root to larger tree root. Decrement component count.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(\alpha(N)) | O(1) |
| Worst | O(\alpha(N)) | O(1) |

Note: Uniting two sets is bounded by two find operations ($O(\alpha(N))$ amortized).

### Traverse
- **How it works:**
  1. Iterate index $i$ from 0 to $N - 1$.
  2. Evaluate `find(i)` to map each element to its canonical root representative.
  3. Group elements into disjoint buckets by root ID.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(N) | O(N) |
| Average | O(N \cdot \alpha(N)) | O(N) |
| Worst | O(N \cdot \alpha(N)) | O(N) |

Note: Identifying all components requires running `find` on all $N$ elements.

### Sort
- **How it works:**
  1. Elements represent equivalence classes without defined total order.
  2. N/A: Sorting is not applicable to disjoint partitions.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | N/A | N/A |
| Average | N/A | N/A |
| Worst | N/A | N/A |

Note: Sorting is N/A because disjoint sets represent algebraic partitions, not ordered sequences.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert (MakeSet) | O(1) | O(1) | O(1) | O(1) |
| Delete (Split) | N/A | N/A | N/A | N/A |
| Search (Find) | O(1) | O(\alpha(N)) | O(\alpha(N)) | O(1) |
| Access (Connected)| O(1) | O(\alpha(N)) | O(\alpha(N)) | O(1) |
| Update (Union) | O(1) | O(\alpha(N)) | O(\alpha(N)) | O(1) |
| Traverse | O(N) | O(N \cdot \alpha(N)) | O(N \cdot \alpha(N)) | O(N) |
| Sort | N/A | N/A | N/A | N/A |

## 3. Variations
- **Standard version:** Weighted Quick-Union with Path Compression. Trade-off: Near-constant $O(\alpha(N))$ time for all operations, but cannot decouple or un-merge elements without rollback tracking.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Quick-Find | `parent[i]` directly stores set ID for every element | O(1) find/connected check | O(N) union operation | Read-heavy static systems with rare unions |
| Quick-Union | Trees with parent links without rank/path optimization | Simple code | O(N) worst-case tree height degeneration | Simple pedagogical illustrations |
| DSU with Rollback | Avoids path compression; logs union modifications in a stack | Supports undo/rollback of union operations | Operations take strict O(log N) instead of O($\alpha(N)$) | Offline dynamic connectivity, divide & conquer on queries |
| Potential / Weighted DSU | Edges store relative distance/weights to parent | Answers relational difference queries between elements | Complex path compression algebra | Parity check, bipartite graphs, modulo relations |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Path Compression + Rank | Flattens tree on `find`; links shorter tree to taller tree | $O(\alpha(N))$ amortized per operation | Two primitive int arrays (`parent[]`, `rank[]`) |
| Path Compression + Size | Tracks component element count `size[]` | $O(\alpha(N))$ amortized; tracks component sizes | Two primitive int arrays (`parent[]`, `size[]`) |
| Path Halving / Splitting | Makes every other node point to its grandparent during find | Single-pass $O(\alpha(N))$ without recursion | Eliminates recursion stack space |

- **Java built-in equivalents:**
  - Standard Java has no built-in `UnionFind` class in `java.util`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyUnionFind.java` | DSU with Path Compression & Union by Rank | Disjoint set with find, union, connected, and component count tracking |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [13 - Graph Representation](../13-graph-representation/README.md) | [15 - Segment Tree](../15-segment-tree/README.md)
