# Connected Components

> Partitioning graphs into mutually reachable subgraphs: flood fill, DSU, and Strongly Connected Components.

## 1. Overview
Connected Components partition graph vertices into maximal subgraphs such that every pair of vertices within a component is mutually reachable. In undirected graphs, components are identified via DFS/BFS flood fill or Union-Find. In directed graphs, Strongly Connected Components (SCCs) where mutual reachability holds in both directions are found using Tarjan's or Kosaraju's algorithm.

## 2. Time & Space Complexity
| Problem / Variant | Algorithm | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Undirected Components | DFS / BFS Flood Fill | O(V + E) | O(V) |
| Undirected Components | Disjoint Set Union (DSU) | O(E * alpha(V)) | O(V) |
| Strongly Connected Components | Tarjan's Algorithm | O(V + E) | O(V) |
| Strongly Connected Components | Kosaraju's Algorithm (2-Pass DFS) | O(V + E) | O(V) |

Tarjan's algorithm finds all SCCs in a single DFS pass using discovery timestamps and low-link values.

## 3. When to Use
- Counting separate islands or disconnected clusters in grids or networks.
- Testing if a graph is fully connected.
- Condensing directed cyclic graphs into DAGs of Strongly Connected Components.
- Finding bridge edges or articulation points (critical connections).

## 4. When NOT to Use
- Graph is already known to be connected.
- Online incremental edge removals (requires advanced dynamic connectivity structures).
- Path distance minimization rather than component partition.

## 5. Why It Works
In undirected graphs, reachability is an equivalence relation (reflexive, symmetric, transitive), partitioning vertices into disjoint equivalence classes. Starting a DFS from any unvisited vertex will exhaustively visit and label all members of its equivalence class.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Pairwise Reachability | Run BFS between every pair of vertices `(u, v)` | O(V * (V + E)) | O(V) |
| Single Pass Flood Fill | Mark entire component during one DFS from each unvisited root | O(V + E) | O(V) |

Flood fill labels entire clusters at once, avoiding individual pairwise reachability searches.

## 7. Data Structures Used Here
- `boolean[] visited`: Marks nodes already assigned to a component.
- `UnionFind`: For dynamic component tracking as edges are added.

## 8. Core Template (Java)
```java
// Count Connected Components in Undirected Graph via DFS
int countComponents(int n, List<List<Integer>> adj) {
    boolean[] visited = new boolean[n];
    int components = 0;
    for (int i = 0; i < n; i++) {
        if (!visited[i]) {
            components++;
            dfsComponent(i, adj, visited);
        }
    }
    return components;
}

void dfsComponent(int u, List<List<Integer>> adj, boolean[] visited) {
    visited[u] = true;
    for (int v : adj.get(u)) {
        if (!visited[v]) dfsComponent(v, adj, visited);
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Cycle Detection](../cycle-detection/README.md) | [Bipartite Graph](../bipartite/README.md)
