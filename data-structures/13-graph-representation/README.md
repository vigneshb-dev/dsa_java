# Graph Representation
> Structural models for representing networks of vertices and edges in memory.

## 1. Fundamentals
- What is it? Non-linear network structures $G = (V, E)$ consisting of a set of vertices (nodes) and connecting edges (directed/undirected, weighted/unweighted).
- What problem does it solve? Models relationships, dependencies, transport road systems, social connections, computer networks, and state transitions.
- What type of data does it store? Vertex identifiers and edge connections with optional weight attributes.
- Linear or non-linear? Non-linear (network structure with arbitrary cycles and branches).
- Static or dynamic? Dynamic: vertices and edges can be added or removed during runtime.
- Ordered or unordered? Unordered sets of vertices and edges; adjacency lists provide local neighbor iteration ordering.
- Mutable or immutable? Mutable in Java: edges, weights, and vertices can be added and modified dynamically.
- How is the data stored internally? Encoded using an Adjacency Matrix (2D array `int[V][V]`), an Adjacency List (`List<Integer>[]` or `Map<V, List<V>>`), or an Edge List (`List<Edge>`).
```text
Graph Representations:
Graph: (0)---(1)---(2)

Adjacency Matrix:              Adjacency List:
     0  1  2                   0 -> [ 1 ]
  0 [0, 1, 0]                  1 -> [ 0, 2 ]
  1 [1, 0, 1]                  2 -> [ 1 ]
  2 [0, 1, 0]
```

## 2. Core Operations

### Insert
- **How it works:**
  1. Add Edge $(u, v)$: In an adjacency matrix, set `matrix[u][v] = weight`. In an adjacency list, append $v$ to `adj.get(u)`.
  2. Add Vertex: In an adjacency list, add a new key or expand array list. In a matrix, reallocate an expanded $(V + 1) \times (V + 1)$ grid.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(1) | O(1) |
| Worst | O(V^2) | O(V^2) |

Note: Edge insertion into an adjacency list is amortized O(1); adding a vertex to a fixed adjacency matrix takes $O(V^2)$ reallocation.

### Delete
- **How it works:**
  1. Remove Edge $(u, v)$: In matrix, set `matrix[u][v] = 0`. In adjacency list, scan and remove $v$ from list `adj[u]`.
  2. Remove Vertex $u$: In matrix, rebuild $(V - 1) \times (V - 1)$ array. In adjacency list, remove $u$'s list and filter $u$ out of all other vertex lists.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(deg(u)) | O(1) |
| Worst | O(V + E) | O(V^2) |

Note: Removing an edge in an adjacency matrix is strictly O(1); in an adjacency list it is $O(\text{deg}(u))$. Removing a vertex takes $O(V + E)$.

### Search
- **How it works:**
  1. Often named `hasEdge(u, v)`.
  2. In an adjacency matrix, directly inspect `matrix[u][v] != 0`.
  3. In an adjacency list, iterate through neighbor list of $u$ to check if $v$ is present.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(deg(u)) | O(1) |
| Worst | O(V) | O(1) |

Note: Adjacency matrix answers edge queries in constant O(1); adjacency list requires scanning neighbor degree $O(\text{deg}(u))$.

### Access
- **How it works:**
  1. Retrieving all adjacent neighbors of vertex $u$.
  2. In an adjacency list, return the list `adj[u]` directly.
  3. In an adjacency matrix, iterate across entire row $u$ checking all $V$ column entries.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(deg(u)) | O(1) |
| Worst | O(V) | O(1) |

Note: Adjacency list accesses neighbors in $O(\text{deg}(u))$ optimal time; adjacency matrix always costs $O(V)$.

### Update
- **How it works:**
  1. Updating edge weight for $(u, v)$.
  2. In matrix: `matrix[u][v] = newWeight`.
  3. In list: find edge $(u, v)$ in neighbor list of $u$ and update weight field.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(1) | O(1) |
| Average | O(deg(u)) | O(1) |
| Worst | O(V) | O(1) |

Note: Updating weight in an adjacency matrix is O(1); updating in a list takes $O(\text{deg}(u))$.

### Traverse
- **How it works:**
  1. Breadth-First Search (BFS): Explore neighbor-by-neighbor using a FIFO queue and a `visited` boolean array.
  2. Depth-First Search (DFS): Recursively or stack-wise explore along each branch until dead-end.
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(V + E) | O(V) |
| Average | O(V + E) | O(V) |
| Worst | O(V^2) | O(V) |

Note: Traversal on adjacency lists is $O(V + E)$; on adjacency matrices it is $O(V^2)$.

### Sort
- **How it works:**
  1. Topological Sort (applicable to Directed Acyclic Graphs):
  2. Compute in-degrees for all vertices; push in-degree 0 nodes to queue.
  3. Pop node, append to sorted order, decrement neighbor in-degrees, and repeat (Kahn's algorithm).
- **Complexity table:**

| Case | Time | Space |
|---|---|---|
| Best | O(V + E) | O(V) |
| Average | O(V + E) | O(V) |
| Worst | O(V + E) | O(V) |

Note: Applicable strictly to DAGs; general graphs cannot be sorted linearly due to potential cycles.

### Summary of Operations
| Operation | Best | Average | Worst | Space |
|---|---|---|---|---|
| Insert Edge (List) | O(1) | O(1) | O(1) | O(1) |
| Delete Edge (List) | O(1) | O(deg(u)) | O(V) | O(1) |
| Search Edge (Matrix)| O(1) | O(1) | O(1) | O(1) |
| Access Neighbors (List)| O(1) | O(deg(u)) | O(V) | O(1) |
| Update Edge | O(1) | O(1) | O(V) | O(1) |
| Traverse (BFS/DFS) | O(V + E) | O(V + E) | O(V + E) | O(V) |
| Sort (Topological) | O(V + E) | O(V + E) | O(V + E) | O(V) |

## 3. Variations
- **Standard version:** Adjacency List (`List<List<Integer>>`). Trade-off: Space-optimal $O(V + E)$ for sparse graphs and fast neighbor iteration, but checking if a specific edge exists takes $O(\text{deg}(u))$.
- **Important variants:**

| Variant | What changes | Pros | Cons | When it is useful |
|---|---|---|---|---|
| Adjacency List | Each vertex stores a list of connected neighbors | Space-optimal $O(V + E)$; fast neighbor iteration | Edge existence check takes $O(\text{deg}(u))$ | Sparse graphs ($E \ll V^2$), standard DSA problems |
| Adjacency Matrix | 2D array `int[V][V]` where cell $(u, v)$ stores edge/weight | $O(1)$ edge existence check; simple matrix math | Consumes $O(V^2)$ space regardless of edge count | Dense graphs ($E \approx V^2$), Floyd-Warshall |
| Edge List | Flat collection of edge tuples `(u, v, weight)` | Minimal space $O(E)$; simple sorting by weight | Finding neighbors takes $O(E)$ full scan | Kruskal's MST, Bellman-Ford algorithm |
| Adjacency Set | Each vertex stores a `HashSet<Integer>` of neighbors | $O(1)$ edge check and space-proportional $O(V + E)$ | Higher constant factor memory per set | Dynamic graphs with frequent edge lookups/deletions |
| Compressed Sparse Row (CSR) | Packed flat arrays storing vertex offsets and neighbor indices | Ultimate cache locality; zero heap fragmentation | Difficult to modify after construction | High-performance graph mining, GPU computing |

- **Implementation differences:**

| Approach | How it works | Time impact | Memory impact |
|---|---|---|---|
| Adjacency List | Array of lists (`List<Integer>[]`) | BFS/DFS runs in $O(V + E)$ | $O(V + E)$ heap memory |
| Adjacency Matrix | Contiguous 2D array `int[V][V]` | BFS/DFS runs in $O(V^2)$ | Rigid $O(V^2)$ memory allocation |
| Adjacency Map | `Map<V, Set<V>>` for arbitrary node types | Handles non-integer vertex keys | Boxing and map hashing overhead |

- **Java built-in equivalents:**
  - Standard Java has no dedicated `Graph` class in `java.util`. Commonly constructed using `ArrayList<ArrayList<Integer>>`, `HashMap<Integer, List<Integer>>`, or 2D primitive arrays `int[][]`.

## 4. Implementation
Own implementations use the name My<Name>.java.

| Done | File | Variant implemented | Notes |
|:---:|---|---|---|
| - [ ] | `MyGraph.java` | Adjacency List & Adjacency Matrix | Graph representations supporting directed/undirected edges, BFS, DFS, and cycle check |

## 5. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [12 - Trie](../12-trie/README.md) | [14 - Union-Find](../14-union-find/README.md)
