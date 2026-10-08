# Cycle Detection

> Identifying cyclic dependencies in directed graphs (3-color DFS) and undirected graphs (DSU / BFS).

## 1. Overview
Cycle Detection determines whether a graph contains one or more closed loop paths starting and ending at the same vertex. In directed graphs, cycles correspond to back-edges in DFS trees, detected via 3-color vertex marking (white = unvisited, gray = visiting, black = finished) or Kahn's algorithm. In undirected graphs, cycles are detected when an edge connects two already connected nodes (using Union-Find or DFS parent tracking).

## 2. Time & Space Complexity
| Graph Type | Algorithm | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Directed Graph | 3-Color DFS | O(V + E) | O(V) call stack |
| Directed Graph | Kahn's Algorithm (in-degree count) | O(V + E) | O(V) queue |
| Undirected Graph | DFS with Parent Pointer | O(V + E) | O(V) |
| Undirected Graph | Union-Find (DSU) | O(E * alpha(V)) | O(V) |

In directed graphs, an edge to an already-visited 'black' node is a cross-edge or forward-edge, NOT a cycle; only edges to 'gray' (currently on stack) nodes indicate cycles.

## 3. When to Use
- Deadlock detection in concurrent transaction wait-for graphs.
- Validating DAG constraints before running topological sort.
- Testing if an undirected graph is a valid tree (connected and acyclic with E = V - 1).
- Detecting circular references in spreadsheets or pointer networks.

## 4. When NOT to Use
- Graph is guaranteed to be a tree by construction.
- Negative cycle detection on weighted graphs (use Bellman-Ford instead).
- Simple single-sequence linked list loops (use Floyd's Tortoise and Hare).

## 5. Why It Works
In directed DFS, the recursion stack contains all ancestors of the currently visited node. Encountering an edge to an active ancestor (gray state) creates a path from ancestor to descendant and back to ancestor, which is a cycle.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Path Tracking DFS | Store all visited nodes in list per path | O(V * (V + E)) | O(V) |
| 3-Coloring DFS | Track 3 states (0=unvisited, 1=visiting, 2=visited) | O(V + E) | O(V) |

3-color states eliminate path copying by distinguishing ancestor recursion stack nodes from completed nodes.

## 7. Data Structures Used Here
- `int[] state`: 0 = unvisited, 1 = visiting (in call stack), 2 = completed.
- `UnionFind`: For incremental edge cycle detection in undirected graphs.

## 8. Core Template (Java)
```java
// 3-Color DFS Cycle Detection in Directed Graph
boolean hasCycleDirected(int u, List<List<Integer>> adj, int[] color) {
    color[u] = 1; // Mark as visiting (gray)
    for (int v : adj.get(u)) {
        if (color[v] == 1) return true; // Back-edge to active ancestor = cycle
        if (color[v] == 0 && hasCycleDirected(v, adj, color)) return true;
    }
    color[u] = 2; // Mark as completed (black)
    return false;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Topological Sort](../topological-sort/README.md) | [Connected Components](../connected-components/README.md)
