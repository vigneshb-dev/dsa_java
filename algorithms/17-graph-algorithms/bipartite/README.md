# Bipartite Graph

> 2-coloring graph vertices such that no two adjacent vertices share the same color.

## 1. Overview
A Bipartite Graph is a graph whose vertices can be divided into two disjoint sets `U` and `V` such that every edge connects a vertex in `U` to a vertex in `V`. Equivalently, a graph is bipartite if and only if it contains no odd-length cycles. Bipartiteness is verified by attempting to 2-color the graph using BFS or DFS; encountering an adjacent node with the same color indicates an odd cycle.

## 2. Time & Space Complexity
| Algorithm / Variant | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| 2-Coloring BFS | O(V + E) | O(V) queue and color array |
| 2-Coloring DFS | O(V + E) | O(V) call stack and color array |
| Hopcroft-Karp Maximum Bipartite Matching | O(E * sqrt(V)) | O(V) |

Graph may be disconnected; every connected component must be independently tested for bipartiteness.

## 3. When to Use
- Partitioning participants or items into two non-conflicting groups (e.g., Possible Bipartition).
- Matching problems (men to women, jobs to workers, students to dorms).
- Detecting presence of odd-length cycles in undirected graphs.
- Testing for 2-colorability in constraint satisfaction problems.

## 4. When NOT to Use
- Graph requires partitioning into 3 or more colors (3-coloring is NP-complete).
- Graph edges have directional flow where maximum flow algorithms apply (e.g., Ford-Fulkerson).
- Problem specifies weighted paths rather than group partitioning.

## 5. Why It Works
If an edge connects two nodes of the same color, the path between them along the tree plus that edge forms a cycle with an odd number of edges. Since odd cycles make 2-coloring impossible, the presence of any monochromatic adjacent edge disproves bipartiteness.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (All 2^V colorings) | Check all 2^V color assignments | O(2^V * (V + E)) | O(V) |
| 2-Coloring BFS / DFS | Propagate alternating colors deterministically | O(V + E) | O(V) |

Color propagation assigns colors deterministically per component, cutting exponential search to linear O(V + E).

## 7. Data Structures Used Here
- `int[] color`: Stores 0 (uncolored), 1 (color A), -1 (color B).
- `ArrayDeque<Integer>`: Queue for BFS 2-coloring.

## 8. Core Template (Java)
```java
// Check Bipartite Graph via BFS 2-Coloring
boolean isBipartite(int[][] graph) {
    int n = graph.length;
    int[] color = new int[n]; // 0: uncolored, 1: blue, -1: red

    for (int i = 0; i < n; i++) {
        if (color[i] != 0) continue;
        Queue<Integer> q = new ArrayDeque<>();
        q.offer(i);
        color[i] = 1;
        while (!q.isEmpty()) {
            int u = q.poll();
            for (int v : graph[u]) {
                if (color[v] == color[u]) return false; // same color neighbor
                if (color[v] == 0) {
                    color[v] = -color[u]; // alternate color
                    q.offer(v);
                }
            }
        }
    }
    return true;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Connected Components](../connected-components/README.md) | End
