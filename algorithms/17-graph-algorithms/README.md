# Graph Algorithms
> Comprehensive paradigms for exploring, routing, partitioning, and analyzing vertex-edge networks.

## 1. Overview
Graph algorithms operate on relational networks $G = (V, E)$ consisting of vertices and connecting edges. They solve essential problems in routing, connectivity, flow optimization, dependency resolution, and network resilience across directed, undirected, weighted, and unweighted graphs.

## 2. Input / Output
- Input: Graph representation (adjacency list `List<List<Integer>>` or matrix `int[][]`) and parameters (source $s$, destination $t$).
- Output: Reachability booleans, shortest path lengths, minimum spanning trees, or topological node orderings.
*Example:* Graph with edges `(0-1, w:4), (1-2, w:2), (0-2, w:8)`; source 0 to 2 $\to$ shortest path distance `6`.

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$ for linear $O(V + E)$ or linearithmic $O(E \log V)$ algorithms.
- Dense graphs ($E \approx V^2$): $V \le 2000$.
- Edge weights can be non-negative, negative, or unweighted. Negative cycles must be detected.

## 4. Brute-Force Approach
- Idea: Exhaustively enumerate all possible simple paths between vertices using unpruned backtracking.
- Pseudocode: Recursive path generation visiting all permutations.
- Time: $O(V!)$ or $O(2^V)$; Space: $O(V)$ recursion depth.

## 5. Optimal Approach
- Idea: Select specialized paradigms matching graph properties: BFS for unweighted shortest paths, Dijkstra for non-negative weights, Kahn's/DFS for topological order, and Union-Find/Prim for MST.
```java
// Generic Graph Traversal Skeleton (Breadth-First Search Template)
import java.util.*;

public class GraphAlgorithmsTemplate {
    public static int[] bfsShortestPath(List<List<Integer>> adj, int startNode, int n) {
        int[] dist = new int[n];
        Arrays.fill(dist, -1); // -1 marks unvisited

        ArrayDeque<Integer> queue = new ArrayDeque<>();
        dist[startNode] = 0;
        queue.offer(startNode);

        while (!queue.isEmpty()) {
            int u = queue.poll();

            for (int v : adj.get(u)) {
                if (dist[v] == -1) { // Unvisited neighbor
                    dist[v] = dist[u] + 1;
                    queue.offer(v);
                }
            }
        }
        return dist;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Avoids re-traversing cycles and re-visiting vertices along suboptimal paths.
- **Cases skipped:** Visited array or distance table skips any vertex whose optimal distance has already been settled.
- **Shortcuts / tricks used:** Greedy selection (Dijkstra PriorityQueue) and FIFO frontier ordering (BFS) guarantee discovery in non-decreasing order of distance.
- **Time saved:** $O(V!) \to O(V + E)$ (unweighted) or $O(E \log V)$ (weighted).
- **Space effect:** Allocates $O(V)$ memory for visited flags, distances, and queues.
- **Trade-off:** Requires memory proportional to graph size ($O(V + E)$).

## 6. Core Idea
Graph exploration maintains a frontier between settled vertices and unexplored territory. Frontier expansion order (FIFO for BFS, LIFO for DFS, priority heap for Dijkstra) strictly dictates the mathematical properties of the traversal.

## 7. Pattern
- Pattern: Graph State Exploration / Network Relaxation.
- Signals: "Shortest path", "connected components", "detect cycle", "course schedule / dependencies", "minimum wire length (MST)", "bipartite coloring".

## 8. Data Structure Used
- Adjacency List `List<List<Integer>>`.
- Boolean array `boolean[] visited` or distance array `int[] dist`.
- `ArrayDeque` for BFS; `PriorityQueue` for Dijkstra/Prim; `UnionFind` for Kruskal.

## 9. Invariant
At step $k$, all settled vertices have their true globally optimal properties (reachability, component ID, or shortest distance) determined and finalized.

## 10. Dry Run
BFS on triangle `0-1, 1-2, 0-2` from source 0:
| Step | Queue State | Dequeued Node | Neighbors Inspected | `dist` Array |
|---|---|---|---|---|
| Init | `[0]` | - | - | `[0, -1, -1]` |
| 1 | `[1, 2]` | 0 | 1 (dist 1), 2 (dist 1) | `[0, 1, 1]` |
| 2 | `[2]` | 1 | 2 (already visited) | `[0, 1, 1]` |
| 3 | `[]` | 2 | None unvisited | `[0, 1, 1]` |

## 11. Edge Cases
- Disconnected components: loop outer vertex counter from $0$ to $V-1$ to ensure all components are visited.
- Self-loops and multi-edges between the same vertex pair.
- Negative cycles in weighted graphs: triggers infinite negative loop (detect with Bellman-Ford).

## 12. Correctness
Proved via induction on path length or edge weights: For BFS, queue ordering ensures nodes at distance $d$ are completely dequeued before any node at distance $d + 1$ is processed. For Dijkstra, non-negative weights ensure popped distances can never be improved.

## 13. Time Complexity
- Unweighted Graph: $O(V + E)$ linear time.
- Weighted Graph: $O(E \log V)$ with min-heap priority queue.
- All-Pairs Shortest Path: $O(V^3)$ via Floyd-Warshall.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for distance, visited, and queue/recursion buffers.

## 15. Can It Be Optimized?
0-1 BFS uses a Deque to solve graphs with edge weights in $\{0, 1\}$ in $O(V + E)$ time without heap log factors. Bidirectional BFS reduces explored states from $O(b^d)$ to $O(b^{d/2})$.

## 16. When Should I Use This Algorithm?
- Path finding and navigation systems.
- Task ordering and compilation dependency resolution (Topological Sort).
- Network routing and latency optimization (Dijkstra / Bellman-Ford).
- Clustering and social network connectivity (Connected Components).
- Circuit design and cable infrastructure planning (MST).

## 17. When Should I NOT Use It?
- Tree graphs where parent-child pointer links provide trivial paths (use Tree Algorithms).
- 2D grid searches where only contiguous straight lines matter (use Two Pointers or Sliding Window).
- Complete brute-force permutation matching with $V > 50$ (NP-hard TSP).

---

## Sub-Topics in This Section
| Sub-folder | Description |
|---|---|
| [traversal](traversal/README.md) | Fundamental network exploration using Breadth-First Search (BFS) and Depth-First Search (DFS) |
| [shortest-path](shortest-path/README.md) | Single-source and all-pairs routing (Dijkstra, Bellman-Ford, Floyd-Warshall, 0-1 BFS) |
| [minimum-spanning-tree](minimum-spanning-tree/README.md) | Connecting all vertices with minimum total edge weight (Kruskal's, Prim's) |
| [topological-sort](topological-sort/README.md) | Linear ordering of vertices in Directed Acyclic Graphs (Kahn's Algorithm, DFS Post-Order) |
| [cycle-detection](cycle-detection/README.md) | Identifying cycles in directed (3-color DFS) and undirected (DSU / BFS) graphs |
| [connected-components](connected-components/README.md) | Identifying isolated components in undirected graphs and strongly connected components (SCC) |
| [bipartite](bipartite/README.md) | Determining 2-colorability and odd cycle absence using BFS/DFS graph coloring |

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic graph algorithm problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [16 - Tree Algorithms](../16-tree-algorithms/README.md) | [18 - String Algorithms](../18-string-algorithms/README.md)
