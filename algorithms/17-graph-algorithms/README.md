# Graph Algorithms

> Core algorithms operating on networks of vertices and edges across directed and undirected graphs.

## 1. Overview
Graph Algorithms solve fundamental connectivity, routing, and ordering problems on network graphs. They encompass breadth-first and depth-first searches, shortest path finding (Dijkstra, Bellman-Ford, Floyd-Warshall), minimum spanning trees (Kruskal, Prim), topological sorting, cycle detection, and bipartite verification. Algorithms are categorized by whether edges are directed/undirected, weighted/unweighted, or cyclic/acyclic.

### Graph Sub-Topic Folders

| Sub-folder | Description |
| :--- | :--- |
| [traversal](traversal/README.md) | BFS for level/shortest paths, DFS for reachability and pathfinding |
| [shortest-path](shortest-path/README.md) | Dijkstra (non-negative weights), Bellman-Ford, and Floyd-Warshall |
| [minimum-spanning-tree](minimum-spanning-tree/README.md) | Kruskal's (with Union-Find) and Prim's (with PriorityQueue) algorithms |
| [topological-sort](topological-sort/README.md) | Kahn's in-degree BFS and DFS post-order reversal for DAG ordering |
| [cycle-detection](cycle-detection/README.md) | Cycle detection via 3-color DFS in directed graphs and DSU in undirected graphs |
| [connected-components](connected-components/README.md) | Counting components, flood fill, and Tarjan's/Kosaraju's SCC algorithms |
| [bipartite](bipartite/README.md) | 2-coloring graphs via BFS/DFS to verify bipartite matching |

## 2. Time & Space Complexity
| Algorithm / Category | Time Complexity | Space Complexity |
| :--- | :--- | :--- |
| BFS / DFS (Adjacency List) | O(V + E) | O(V) |
| Dijkstra's (with Min-Heap) | O((V + E) log V) | O(V) |
| Bellman-Ford | O(V * E) | O(V) |
| Floyd-Warshall (All-Pairs) | O(V^3) | O(V^2) |
| Kruskal's MST (with DSU) | O(E log E) | O(V) |
| Kahn's Topological Sort | O(V + E) | O(V) |

For dense graphs where E ~= V^2, Dijkstra with min-heap takes O(V^2 log V), whereas an array-based Dijkstra takes O(V^2).

## 3. When to Use
- Modeling real-world networks (road maps, social connections, computer routing).
- Dependency resolution, build task sequencing, or course prerequisites (Topological Sort).
- Finding shortest path or minimum latency routing between nodes.
- Network connectivity, clustering, or partitioning components.

## 4. When NOT to Use
- Graph is guaranteed to be a tree (simpler tree traversals avoid visited set overhead).
- Data is linear or grid without arbitrary interconnects.
- Vertices and edges are implicit and unbounded.

## 5. Why It Works
Graph algorithms track state (e.g., `visited` sets, `dist` arrays, `inDegree` counts) to ensure each vertex and edge is processed a bounded number of times. Greedy relaxations (Dijkstra) or FIFO frontiers (BFS) guarantee that optimal distances are finalized before processing further.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (All Paths Search) | Enumerate all paths between source and destination | O(V!) | O(V) |
| Optimized (BFS / Dijkstra) | Explore boundary frontiers monotonically | O(V + E) or O((V+E) log V) | O(V) |

Shortest-path algorithms prune suboptimal paths immediately upon discovering a cheaper route to any intermediate vertex.

## 7. Data Structures Used Here
- `List<List<Integer>>`: Adjacency list representation.
- `ArrayDeque<Integer>`: Queue for BFS and Kahn's algorithm.
- `PriorityQueue<int[]>`: Min-heap storing `[distance, vertex]` pairs for Dijkstra.

## 8. Core Template (Java)
```java
// Standard BFS Traversal template on Adjacency List
void bfs(int start, List<List<Integer>> adj, int n) {
    boolean[] visited = new boolean[n];
    Queue<Integer> queue = new ArrayDeque<>();
    visited[start] = true;
    queue.offer(start);

    while (!queue.isEmpty()) {
        int u = queue.poll();
        for (int v : adj.get(u)) {
            if (!visited[v]) {
                visited[v] = true;
                queue.offer(v);
            }
        }
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Tree Algorithms](../16-tree-algorithms/README.md) | [String Algorithms](../18-string-algorithms/README.md)
