# Minimum Spanning Tree
> Connecting all vertices in an undirected weighted graph with minimum total edge weight and zero cycles.

## 1. Overview
A Minimum Spanning Tree (MST) is an acyclic subgraph connecting all vertices of an undirected connected weighted graph with the minimum possible total edge weight. The two classic greedy approaches are Kruskal's Algorithm (sort edges and unite components via Union-Find) and Prim's Algorithm (grow a connected cut component via a priority queue).

## 2. Input / Output
- Input: An undirected weighted connected graph with $V$ vertices and $E$ edges.
- Output: The list of $V - 1$ tree edges or total minimum weight sum.

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- Graph must be undirected and connected (otherwise produces a Minimum Spanning Forest).
- Edge weights can be negative, zero, or positive.

## 4. Brute-Force Approach
- Idea: Enumerate all possible spanning trees ($V^{V-2}$ by Cayley's formula) and select the one with minimum sum.
- Pseudocode: Combinatorial spanning tree generation.
- Time: $O(V^{V-2})$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: The Cut Property guarantees that the minimum weight edge crossing any cut in the graph belongs to the MST.
```java
// Reusable MST Skeleton (Kruskal's Algorithm Template)
import java.util.*;

public class MinimumSpanningTreeTemplate {
    static class Edge {
        int u, v, weight;
        Edge(int u, int v, int w) { this.u = u; this.v = v; this.weight = w; }
    }

    public static int kruskalMST(int n, List<Edge> edges) {
        // 1. Sort all edges by weight ascending
        Collections.sort(edges, (a, b) -> Integer.compare(a.weight, b.weight));

        // 2. Disjoint Set Union (DSU) to prevent cycles
        int[] parent = new int[n];
        for (int i = 0; i < n; i++) parent[i] = i;

        int totalWeight = 0;
        int edgesCount = 0;

        for (Edge edge : edges) {
            int rootU = find(parent, edge.u);
            int rootV = find(parent, edge.v);

            if (rootU != rootV) {
                parent[rootU] = rootV; // Unite components
                totalWeight += edge.weight;
                edgesCount++;
                if (edgesCount == n - 1) break; // MST complete
            }
        }
        return totalWeight;
    }

    private static int find(int[] parent, int i) {
        if (parent[i] == i) return i;
        return parent[i] = find(parent, parent[i]); // Path compression
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Bypasses exploring cyclic subgraphs or checking exponential tree candidates.
- **Cases skipped:** If an edge connects two vertices already in the same connected component, it is discarded in $O(\alpha(V))$ time without further exploration.
- **Shortcuts / tricks used:** Greedy selection ordered by edge weight + Union-Find cycle check.
- **Time saved:** $O(V^{V-2}) \to O(E \log E)$.
- **Space effect:** Allocates $O(V)$ DSU parent array and sorted edge list.
- **Trade-off:** Kruskal requires sorting all edges; Prim requires priority queue overhead.

## 6. Core Idea
The Cut Property: For any cut partitioning vertices into sets $S$ and $V \setminus S$, the lightest edge crossing the cut boundary must belong to the Minimum Spanning Tree.

## 7. Pattern
- Pattern: Greedy Cut Selection / Disjoint Set Kruskal.
- Signals: "Connect all cities with minimum cable length", "minimum cost to connect points (Manhattan/Euclidean)", "network redundancy elimination".

## 8. Data Structure Used
- Kruskal: Disjoint Set Union (Union-Find) with path compression.
- Prim: `PriorityQueue` (Min-Heap) and `boolean[] inMST`.

## 9. Invariant
At every step of Kruskal's or Prim's algorithm, the set of selected edges forms a valid forest that is a subgraph of some minimum spanning tree.

## 10. Dry Run
Kruskal on edges `(1-2: 1), (2-3: 2), (1-3: 4)`:
| Edge | Weight | Component Roots | Action | Total MST Weight |
|---|---|---|---|---|
| (1, 2) | 1 | root(1) != root(2) | Include edge; unite 1 & 2 | 1 |
| (2, 3) | 2 | root(2) != root(3) | Include edge; unite 2 & 3 | $1 + 2 = 3$ |
| (1, 3) | 4 | root(1) == root(3) | Cycle detected! Skip edge | 3 |

MST complete with $V - 1 = 2$ edges and total weight 3.

## 11. Edge Cases
- Disconnected graph: cannot form a spanning tree ($edgesCount < V - 1$ at end).
- Duplicate edge weights: algorithm handles stably; multiple valid MSTs with identical total weight may exist.
- Self-loops: DSU naturally skips since both endpoints share the same root.

## 12. Correctness
Proved via Cut Property and Exchange Argument: If an MST $T$ does not contain light edge $e$, adding $e$ creates a cycle containing some heavier edge $e'$ crossing the same cut. Replacing $e'$ with $e$ yields a tree with weight $\le weight(T)$, establishing that $e$ is part of an optimal MST.

## 13. Time Complexity
- Kruskal: $O(E \log E)$ (dominated by sorting edges).
- Prim: $O(E \log V)$ with binary heap; $O(V^2)$ with adjacency matrix on dense graphs.

## 14. Space Complexity
- Auxiliary Space: $O(V + E)$ for DSU or priority queue buffers.

## 15. Can It Be Optimized?
Borůvka's algorithm achieves $O(E \log V)$ and is easily parallelizable. For dense graphs ($E \approx V^2$), dense Prim runs in $O(V^2)$ without heap overhead.

## 16. When Should I Use This Algorithm?
- Designing physical electrical, water, or internet communication networks.
- Approximating the Metric Traveling Salesperson Problem (2-approximation).
- Single-linkage hierarchical clustering in machine learning.
- Connecting points in 2D space with minimum Manhattan wire distance.

## 17. When Should I NOT Use It?
- Directed graphs (use Chu-Liu/Edmonds algorithm for minimum branching arborescence).
- Shortest path from a single source (MST minimises total edge sum, not path distance; use Dijkstra).
- Bounded degree spanning trees (NP-hard).

---

## Comparison Table
| Algorithm | Time | Space | Graph Type | Best Use |
|---|---|---|---|---|
| Kruskal's | $O(E \log E)$ | $O(V + E)$ | Sparse ($E \ll V^2$) | Edge list available, simple DSU logic |
| Prim's (Heap) | $O(E \log V)$ | $O(V + E)$ | Sparse to Medium | Adjacency list, growing connected tree |
| Prim's (Dense) | $O(V^2)$ | $O(V)$ | Dense ($E \approx V^2$) | Complete graphs, geometric point sets |

---

### Algorithm: Kruskal's Algorithm
- **Input / Output:** Edge list of weighted undirected graph $\to$ MST weight and edge set.
- **Constraints:** $V \le 10^5, E \le 2 \times 10^5$.
- **Brute Force:** Enumerate spanning trees ($O(V^{V-2})$).
- **Optimal Approach:** Sort all edges ascending; unite endpoints using DSU if they belong to different components.
- **How It Reduces Time/Space:** Fast DSU prevents cycle checks from taking $O(V)$ per edge.
- **Core Idea:** Greedily add the globally lightest edge that does not form a cycle.
- **Pattern:** Greedy edge sorting + Disjoint Set Union.
- **Data Structure Used:** `List<Edge>`, DSU (`parent[]`).
- **Invariant:** Forest of trees grows until forming a single spanning tree.
- **Dry Run:** Sorts edges, iterates, accepts edges connecting disjoint sets.
- **Edge Cases:** Disconnected graphs fail to reach $V - 1$ edges.
- **Correctness:** Cut property applied to components united by lightest edge.
- **Time Complexity:** $O(E \log E) = O(E \log V)$.
- **Space Complexity:** $O(V)$ DSU space.
- **Can It Be Optimized:** Linear time if edges are pre-sorted or bounded integers.
- **When to Use:** Sparse graphs; edge list already provided.
- **When NOT to Use:** Dense complete graphs (use Dense Prim).

### Algorithm: Prim's Algorithm
- **Input / Output:** Adjacency list $\to$ MST weight.
- **Constraints:** $V \le 10^5, E \le 2 \times 10^5$.
- **Brute Force:** Enumerate cuts and evaluate all combinations.
- **Optimal Approach:** Start at arbitrary vertex; use PriorityQueue to repeatedly add lightest edge leaving current tree.
- **How It Reduces Time/Space:** Keeps a single connected component, expanding radially.
- **Core Idea:** Grow tree node by node by picking the cheapest cut edge.
- **Pattern:** Greedy cut expansion via PriorityQueue.
- **Data Structure Used:** `PriorityQueue<int[]>`, `boolean[] inMST`, `int[] minWeight`.
- **Invariant:** Maintained subtree is always connected.
- **Dry Run:** Adds node 0 to tree, pushes incident edges to heap, pops min, repeats.
- **Edge Cases:** Starting from disconnected vertex.
- **Correctness:** Direct application of Cut Property on the cut separating tree nodes from non-tree nodes.
- **Time Complexity:** $O(E \log V)$ (Heap) or $O(V^2)$ (Dense array).
- **Space Complexity:** $O(V + E)$.
- **Can It Be Optimized:** Fibonacci heap achieves $O(E + V \log V)$.
- **When to Use:** Dense graphs ($O(V^2)$ version); growing connected tree from specific node.
- **When NOT to Use:** Sparse graphs with raw edge lists (Kruskal is simpler).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic minimum spanning tree problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Shortest Path](../shortest-path/README.md) | [Topological Sort](../topological-sort/README.md)
