# Graph Representation

> Techniques for storing vertices and edges: adjacency lists, adjacency matrices, and edge lists.

## 1. Overview
A Graph is a mathematical structure consisting of vertices (nodes) and edges connecting pairs of vertices. How a graph is represented in memory determines the space overhead and efficiency of neighbor iteration and edge queries. The most common representations are Adjacency Lists (optimal for sparse graphs) and Adjacency Matrices (convenient for dense graphs).

## 2. Time & Space Complexity
| Representation | Space | Check Edge (u, v) | Iterate Neighbors of u |
| :--- | :--- | :--- | :--- |
| Adjacency Matrix | O(V^2) | O(1) | O(V) |
| Adjacency List | O(V + E) | O(deg(u)) | O(deg(u)) |
| Edge List | O(E) | O(E) | O(E) |

In sparse graphs where E << V^2, an Adjacency List saves significant memory and allows neighbor iteration in output-sensitive O(deg(u)) time.

## 3. When to Use
- Adjacency List: Sparse graphs, standard BFS/DFS, Dijkstra, topological sort.
- Adjacency Matrix: Dense graphs (E ~= V^2), small vertex count (V <= 500), Floyd-Warshall algorithm.
- Edge List: Kruskal's Minimum Spanning Tree or Bellman-Ford where algorithms process edges globally.

## 4. When NOT to Use
- Adjacency Matrix on large sparse graphs (e.g., V = 100,000 creates an array exceeding memory limits).
- Edge List when neighbor lookups are needed inside tight inner loops (linear scan over E is slow).
- Over-complicating tree problems where parent-child pointer nodes suffice.

## 5. Why It Works
Adjacency lists store only existing connections, avoiding the O(V^2) memory footprint of empty non-edges. Java arrays of lists `List<Integer>[]` provide direct O(1) vertex indexing combined with sequential neighbor iteration.

## 6. Brute Force vs Optimized
| Approach | Idea | Time (Iterate Neighbors) | Space |
| :--- | :--- | :--- | :--- |
| Adjacency Matrix on Sparse Graph | Iterate through all V columns to find connected neighbors | O(V) | O(V^2) |
| Adjacency List on Sparse Graph | Iterate directly through list of degree d neighbors | O(deg(u)) | O(V + E) |

Adjacency lists compress empty edge entries into dynamic lists, reducing iteration and storage costs.

## 7. Data Structures Used Here
- `List<List<Integer>>` or `List<Integer>[]`: Standard adjacency list in Java.
- `int[][]`: 2D array representation for adjacency matrices.
- `Map<Integer, List<Integer>>`: Adjacency list when vertex identifiers are non-contiguous or sparse.

## 8. Core Template (Java)
```java
// Adjacency List construction for n vertices
int n = 5;
int[][] edges = {{0, 1}, {0, 2}, {1, 3}, {2, 4}};
List<List<Integer>> adj = new ArrayList<>(n);
for (int i = 0; i < n; i++) {
    adj.add(new ArrayList<>());
}
for (int[] edge : edges) {
    int u = edge[0], v = edge[1];
    adj.get(u).add(v);
    adj.get(v).add(u); // omit for directed graph
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Trie](../12-trie/README.md) | [Union-Find (Disjoint Set Union)](../14-union-find/README.md)
