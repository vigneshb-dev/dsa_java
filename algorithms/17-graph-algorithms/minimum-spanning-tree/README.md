# Minimum Spanning Tree

> Connecting all vertices with minimal total edge weight: Kruskal's and Prim's algorithms.

## 1. Overview
A Minimum Spanning Tree (MST) is a subset of edges in a connected, edge-weighted undirected graph that connects all V vertices together without any cycles, minimizing total edge weight. Kruskal's algorithm sorts all edges and greedily adds the cheapest edge that does not form a cycle using Union-Find. Prim's algorithm grows a single tree outward from an arbitrary root by greedily adding the cheapest boundary edge using a priority queue.

## 2. Time & Space Complexity
| Algorithm | Graph Suitability | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Kruskal's Algorithm | Sparse graphs (E << V^2) | O(E log E) | O(V) for DSU |
| Prim's Algorithm (Binary Heap) | Dense graphs (E ~= V^2) | O((V + E) log V) | O(V) |
| Prim's Algorithm (Adjacency Matrix) | Dense graphs (V <= 1000) | O(V^2) | O(V) |

Because E <= V^2, log(E) is at most 2 * log(V), making O(E log E) equivalent to O(E log V) asymptotically.

## 3. When to Use
- Connecting all network terminals, cities, or electrical points with minimal total wiring/cable cost.
- Clustering algorithms (Single-Linkage hierarchical clustering).
- Approximating NP-hard Metric Traveling Salesperson problems.
- Minimizing max-weight bottleneck edges across networks.

## 4. When NOT to Use
- Graph is directed (requires Arborescence / Edmond's algorithm).
- Graph is disconnected (MST does not exist; yields a Minimum Spanning Forest).
- Targeting shortest point-to-point path rather than total network connection weight (use Dijkstra).

## 5. Why It Works
The Cut Property proves that for any partition cut of vertices into sets S and V-S, the minimum weight edge crossing the cut must belong to some MST. Both Kruskal's and Prim's algorithms greedily select cut-crossing edges, guaranteeing optimal spanning trees.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Spanning Trees) | Check all V^(V-2) spanning trees (Cayley's formula) | O(V^V) | O(V) |
| Kruskal's with Union-Find | Sort edges and add non-cyclic edges via DSU | O(E log E) | O(V) |

Greedy cut property selection avoids combinatorial spanning tree enumeration.

## 7. Data Structures Used Here
- `UnionFind`: For cycle detection in Kruskal's algorithm.
- `PriorityQueue<int[]>`: For cheapest boundary edge retrieval in Prim's algorithm.

## 8. Core Template (Java)
```java
// Kruskal's MST Algorithm with Union-Find
int kruskalMST(int n, int[][] edges) { // edges: [u, v, weight]
    Arrays.sort(edges, (a, b) -> Integer.compare(a[2], b[2]));
    UnionFind uf = new UnionFind(n);
    int mstWeight = 0, edgesCount = 0;

    for (int[] edge : edges) {
        if (uf.union(edge[0], edge[1])) {
            mstWeight += edge[2];
            edgesCount++;
            if (edgesCount == n - 1) break;
        }
    }
    return edgesCount == n - 1 ? mstWeight : -1;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Shortest Path](../shortest-path/README.md) | [Topological Sort](../topological-sort/README.md)
