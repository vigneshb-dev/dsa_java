# Shortest Path

> Finding minimal weight paths: Dijkstra, Bellman-Ford, and Floyd-Warshall algorithms.

## 1. Overview
Shortest Path algorithms find the route between vertices that minimizes the sum of constituent edge weights. Dijkstra's algorithm uses a priority queue for non-negative weights; Bellman-Ford handles graphs with negative weights and detects negative cycles; Floyd-Warshall calculates all-pairs shortest paths via dynamic programming. Choosing the proper algorithm depends on edge weight signs and graph density.

## 2. Time & Space Complexity
| Algorithm | Edge Weights | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Dijkstra (PriorityQueue) | Non-negative only | O((V + E) log V) | O(V) |
| Bellman-Ford | Negative allowed | O(V * E) | O(V) |
| Floyd-Warshall | Negative allowed (no neg cycles) | O(V^3) | O(V^2) |
| 0-1 BFS | Weights only 0 or 1 | O(V + E) | O(V) |

Dijkstra fails on graphs with negative edge weights because its greedy assumption (that visited nodes have finalized shortest distances) is invalidated.

## 3. When to Use
- Dijkstra: Single-source shortest path with non-negative edge weights.
- 0-1 BFS: Edges have weights of only 0 or 1 (uses `ArrayDeque.addFirst` / `addLast`).
- Bellman-Ford: Edge weights can be negative, or negative cycles must be detected.
- Floyd-Warshall: Small graphs (V <= 400) requiring all-pairs distances.

## 4. When NOT to Use
- Edges are completely unweighted (use simple BFS for O(V + E) instead of O((V+E) log V)).
- Graph is a Directed Acyclic Graph (DAG) (topological sort relaxation runs in linear O(V + E) time).
- Negative edge cycles exist on paths between source and target (shortest path is undefined / -infinity).

## 5. Why It Works
Dijkstra's algorithm works by greedily extracting the vertex with the smallest provisional distance. Because edge weights are non-negative, taking further edges from any other candidate cannot produce a shorter path to this vertex, finalizing its distance.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| DFS Path Enumeration | Explore all possible paths from source to target | O(V!) | O(V) |
| Dijkstra's Algorithm | Relax neighbors greedily via Min-Heap | O((V + E) log V) | O(V) |

Dijkstra eliminates exponential path permutations by finalizing shortest distances monotonically.

## 7. Data Structures Used Here
- `PriorityQueue<int[]>`: Min-heap storing `[dist, node]` pairs.
- `int[] dist`: Distance array initialized to infinity.

## 8. Core Template (Java)
```java
// Dijkstra's Shortest Path Skeleton
int[] dijkstra(int start, List<List<int[]>> adj, int n) {
    int[] dist = new int[n];
    Arrays.fill(dist, Integer.MAX_VALUE);
    dist[start] = 0;
    PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[0], b[0]));
    pq.offer(new int[]{0, start});

    while (!pq.isEmpty()) {
        int[] curr = pq.poll();
        int d = curr[0], u = curr[1];
        if (d > dist[u]) continue;
        for (int[] edge : adj.get(u)) {
            int v = edge[0], weight = edge[1];
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                pq.offer(new int[]{dist[v], v});
            }
        }
    }
    return dist;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Graph Traversal](../traversal/README.md) | [Minimum Spanning Tree](../minimum-spanning-tree/README.md)
