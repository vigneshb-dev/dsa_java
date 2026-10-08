# Union-Find (Disjoint Set Union)

> Tracks partitioned sets with near-constant time union and find operations using path compression.

## 1. Overview
Union-Find, or Disjoint Set Union (DSU), maintains a collection of disjoint partitions across a universe of elements. It provides two primary operations: `find`, which returns the canonical representative of an element's set, and `union`, which merges two sets. With path compression and union by rank/size, operations run in virtually constant amortized time O(alpha(n)).

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| `find(x)` (with path compression) | O(1) / O(alpha(n)) / O(alpha(n)) | O(1) iterative |
| `union(x, y)` (with rank/size) | O(1) / O(alpha(n)) / O(alpha(n)) | O(1) |
| `connected(x, y)` | O(1) / O(alpha(n)) / O(alpha(n)) | O(1) |
| Initialize DSU of size n | O(n) / O(n) / O(n) | O(n) |

alpha(n) is the inverse Ackermann function, which is strictly less than 5 for all practical input sizes (n <= 10^80).

## 3. When to Use
- Detecting cycles in undirected graphs as edges are processed incrementally.
- Finding number of connected components in dynamic graphs.
- Kruskal's Minimum Spanning Tree algorithm.
- Accounts merge, friend circles, or percolation cluster connectivity.

## 4. When NOT to Use
- Directed graphs where reachability is asymmetric (DSU models symmetric equivalence relations).
- Splitting sets or deleting edges dynamically (standard DSU only supports merging; edge deletions require offline divide-and-conquer).
- Finding shortest path or edge-weighted distances between nodes (use BFS or Dijkstra).

## 5. Why It Works
Path compression flattens tree depth during `find` calls by pointing visited nodes directly to the root. Union by rank attaches shallower trees underneath deeper tree roots, preventing tree height from growing beyond O(log n) even before compression.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (BFS/DFS per query) | Run full graph search to test connectivity between u and v | O(V + E) per query | O(V) |
| Optimized (Union-Find) | Test if `find(u) == find(v)` | O(alpha(n)) ~ O(1) | O(n) |

Union-Find maintains representative canonical roots so dynamic connectivity queries run in amortized constant time.

## 7. Data Structures Used Here
- `int[] parent`: Array where `parent[i]` stores the parent node of `i`.
- `int[] rank` / `int[] size`: Array tracking tree depth bound or component sizes.

## 8. Core Template (Java)
```java
// Standard Union-Find implementation with Path Compression & Union by Rank
class UnionFind {
    private final int[] parent;
    private final int[] rank;
    private int count;

    public UnionFind(int n) {
        this.count = n;
        parent = new int[n];
        rank = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;
    }

    public int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // path compression
        }
        return parent[x];
    }

    public boolean union(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return false; // already connected
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        count--;
        return true;
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Graph Representation](../13-graph-representation/README.md) | [Segment Tree](../15-segment-tree/README.md)
