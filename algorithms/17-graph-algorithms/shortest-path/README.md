# Shortest Path
> Finding minimal weight paths between vertices across weighted and unweighted graphs.

## 1. Overview
Shortest Path algorithms determine the path of minimum cumulative edge weight connecting source and destination vertices. The optimal approach depends on graph characteristics: Dijkstra for non-negative weights, Bellman-Ford for graphs with negative weights or negative cycle detection, Floyd-Warshall for all-pairs, and 0-1 BFS for binary $\{0, 1\}$ weights.

## 2. Input / Output
- Input: Weighted graph and source $s$ (e.g. edge list with weights, source 0).
- Output: Array `dist` where `dist[u]` is minimum path distance from $s$ to $u$.

## 3. Constraints
- Dijkstra: Non-negative edge weights ($w \ge 0$), $V \le 10^5, E \le 2 \times 10^5$.
- Bellman-Ford: Arbitrary edge weights, $V \le 2000, E \le 10^4$.
- Floyd-Warshall: All pairs, $V \le 500$ ($O(V^3)$).
- 0-1 BFS: Edge weights in $\{0, 1\}$, $V \le 10^5$.

## 4. Brute-Force Approach
- Idea: Explore all possible simple paths from $s$ to $t$ using DFS and pick the minimum sum.
- Pseudocode: Unpruned recursive path search.
- Time: $O(V!)$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: Edge relaxation: if `dist[u] + weight < dist[v]`, update `dist[v]`. Order of relaxations is governed by priority queue (Dijkstra) or edge iterations (Bellman-Ford).
```java
// Reusable Shortest Path Skeleton (Dijkstra's Algorithm Template)
import java.util.*;

public class ShortestPathTemplate {
    static class Edge {
        int to, weight;
        Edge(int t, int w) { this.to = t; this.weight = w; }
    }

    public static int[] dijkstra(List<List<Edge>> adj, int start, int n) {
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[start] = 0;

        // Min-Heap ordered by cumulative distance: [node, currentDist]
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> Integer.compare(a[1], b[1]));
        pq.offer(new int[]{start, 0});

        while (!pq.isEmpty()) {
            int[] curr = pq.poll();
            int u = curr[0], d = curr[1];

            // Stale entry check (skip if shorter path already settled)
            if (d > dist[u]) continue;

            for (Edge edge : adj.get(u)) {
                int v = edge.to;
                if (dist[u] + edge.weight < dist[v]) {
                    dist[v] = dist[u] + edge.weight; // Edge relaxation
                    pq.offer(new int[]{v, dist[v]});
                }
            }
        }
        return dist;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Suboptimal and longer paths to a vertex are discarded immediately via `if (d > dist[u]) continue`.
- **Cases skipped:** Once a node's minimum distance is popped from the priority queue in Dijkstra, it is permanently finalized.
- **Shortcuts / tricks used:** Greedy selection extracts the globally smallest unvisited tentative distance first.
- **Time saved:** $O(V!) \to O(E \log V)$ for Dijkstra.
- **Space effect:** Allocates $O(V)$ distance array and priority queue.
- **Trade-off:** Dijkstra strictly requires non-negative weights ($w \ge 0$).

## 6. Core Idea
Repeatedly relax edges ($dist[v] = \min(dist[v], dist[u] + weight)$). Greedy priority queues or topological relaxations guarantee paths settle in optimal order.

## 7. Pattern
- Pattern: Edge Relaxation / Greedy Frontier.
- Signals: "Shortest travel time", "minimum cost path", "network delay time", "cheapest flights with k stops", "arbitrage opportunity (negative cycle)".

## 8. Data Structure Used
- `PriorityQueue` (Min-Heap) for Dijkstra.
- `ArrayDeque` for 0-1 BFS.
- 2D matrix `int[V][V]` for Floyd-Warshall.

## 9. Invariant
In Dijkstra, when vertex $u$ is popped from the priority queue, `dist[u]` is mathematically guaranteed to be the true shortest path distance from source to $u$.

## 10. Dry Run
Dijkstra on `(0-1, w:2), (0-2, w:5), (1-2, w:1)` from 0:
| Step | Polled Node | Polled Dist | Relaxations | `dist` Array |
|---|---|---|---|---|
| Init | - | - | - | `[0, inf, inf]` |
| 1 | Node 0 | 0 | $v=1 (2), v=2 (5)$ | `[0, 2, 5]` |
| 2 | Node 1 | 2 | $v=2 (2 + 1 = 3 < 5)$ | `[0, 2, 3]` |
| 3 | Node 2 | 3 | No outgoing | `[0, 2, 3]` |

## 11. Edge Cases
- Negative weight edges: Dijkstra fails; use Bellman-Ford.
- Negative cycles: cause infinite distance reductions; Bellman-Ford detects via $(V)$-th relaxation check.
- Unreachable vertices: `dist[v] == Integer.MAX_VALUE`.

## 12. Correctness
Dijkstra: By induction on settled vertices. Because all weights are $\ge 0$, any alternative path to the next closest vertex must pass through an unvisited node with distance $\ge$ current minimum, which cannot yield a shorter total length.

## 13. Time Complexity
- Dijkstra: $O(E \log V)$.
- 0-1 BFS: $O(V + E)$.
- Bellman-Ford: $O(V \cdot E)$.
- Floyd-Warshall: $O(V^3)$.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for single-source; $O(V^2)$ for Floyd-Warshall.

## 15. Can It Be Optimized?
Fibonacci Heaps theoretically reduce Dijkstra to $O(E + V \log V)$. On DAGs, topological sort solves shortest path in linear $O(V + E)$ even with negative weights.

## 16. When Should I Use This Algorithm?
- GPS route navigation and map direction finding.
- Network packet routing latency minimization.
- Arbitrage currency trading detection (Bellman-Ford on log exchange rates).
- All-pairs transitive closure or distance matrices (Floyd-Warshall).
- Games on grids with 0 or 1 move costs (0-1 BFS).

## 17. When Should I NOT Use It?
- Unweighted graphs (use simple BFS in $O(V + E)$).
- Trees (use Tree LCA and depth arithmetic in $O(1)$).
- DAGs (use DAG DP in linear $O(V + E)$ time).

---

## Comparison Table
| Algorithm | Time | Space | Negative Weights? | Best Use |
|---|---|---|---|---|
| Dijkstra | $O(E \log V)$ | $O(V)$ | No | Single-source, non-negative weights |
| 0-1 BFS | $O(V + E)$ | $O(V)$ | Weights in $\{0, 1\}$ only | Binary cost grids and graphs |
| Bellman-Ford | $O(V \cdot E)$ | $O(V)$ | Yes | Negative weights, negative cycle detection |
| Floyd-Warshall | $O(V^3)$ | $O(V^2)$ | Yes (no negative cycles) | All-pairs shortest paths, dense small graphs ($V \le 500$) |

---

### Algorithm: Dijkstra's Algorithm
- **Input / Output:** Weighted graph ($w \ge 0$) and source $s$ $\to$ Array of shortest distances.
- **Constraints:** $V \le 10^5, E \le 2 \times 10^5$, non-negative edge weights.
- **Brute Force:** DFS paths search ($O(V!)$).
- **Optimal Approach:** PriorityQueue extracting smallest tentative distance, relaxing incident edges.
- **How It Reduces Time/Space:** Settles vertices greedily; stale entries pruned in $O(1)$.
- **Core Idea:** The shortest path to the nearest unvisited node cannot be improved via any other path.
- **Pattern:** Greedy state relaxation via min-heap.
- **Data Structure Used:** `PriorityQueue<int[]>`, `int[] dist`.
- **Invariant:** Settled nodes popped from heap have final shortest distances.
- **Dry Run:** Repeatedly pops minimum distance node and updates neighbor distances.
- **Edge Cases:** Disconnected nodes remain $\infty$; skip stale entries with `d > dist[u]`.
- **Correctness:** Exchange argument based on non-negative edge weights.
- **Time Complexity:** $O(E \log V)$.
- **Space Complexity:** $O(V)$ queue and distance table.
- **Can It Be Optimized:** Fibonacci heap achieves $O(E + V \log V)$.
- **When to Use:** Single-source shortest path with non-negative weights.
- **When NOT to Use:** Graphs with negative edges (use Bellman-Ford).

### Algorithm: Bellman-Ford Algorithm
- **Input / Output:** Weighted graph (negative weights allowed) $\to$ Shortest paths or negative cycle flag.
- **Constraints:** $V \le 2500, E \le 10^4$.
- **Brute Force:** Path search ($O(V!)$).
- **Optimal Approach:** Relax all $E$ edges $V - 1$ times; check for updates on $V$-th pass.
- **How It Reduces Time/Space:** Systematic DP relaxation bounds path length to $V - 1$ edges.
- **Core Idea:** A simple shortest path contains at most $V - 1$ edges.
- **Pattern:** Dynamic Programming / Edge Relaxation.
- **Data Structure Used:** `int[] dist`, edge list `List<Edge>`.
- **Invariant:** After $k$ rounds, `dist` stores shortest paths containing at most $k$ edges.
- **Dry Run:** Iterates all edges $V - 1$ times; updates propagate along paths.
- **Edge Cases:** Negative cycles detected if any edge relaxes on $V$-th iteration.
- **Correctness:** By induction on path edge count.
- **Time Complexity:** $O(V \cdot E)$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** SPFA (Shortest Path Faster Algorithm) uses queue to relax only modified nodes.
- **When to Use:** Graphs with negative weights; detecting negative cycles.
- **When NOT to Use:** Non-negative graphs (Dijkstra is much faster).

### Algorithm: Floyd-Warshall Algorithm
- **Input / Output:** Graph adjacency matrix $\to$ All-pairs shortest path matrix `dist[V][V]`.
- **Constraints:** $V \le 500$.
- **Brute Force:** Run Dijkstra from all $V$ vertices ($O(V \cdot E \log V)$).
- **Optimal Approach:** Triple nested loop iterating intermediate vertex $k$: `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`.
- **How It Reduces Time/Space:** DP over allowed intermediate vertices $0 \dots k$.
- **Core Idea:** Shortest path between $i$ and $j$ either uses $k$ or does not.
- **Pattern:** All-Pairs DP.
- **Data Structure Used:** 2D matrix `int[V][V]`.
- **Invariant:** After round $k$, `dist[i][j]` is shortest path using intermediate vertices from $\{0 \dots k\}$.
- **Dry Run:** Updates matrix cell by cell through candidate intermediate hubs.
- **Edge Cases:** Negative cycles detected if `dist[i][i] < 0`.
- **Correctness:** Substructure over prefix sets of intermediate nodes.
- **Time Complexity:** $\Theta(V^3)$.
- **Space Complexity:** $\Theta(V^2)$.
- **Can It Be Optimized:** Johnson's algorithm achieves $O(V^2 \log V + V E)$ on sparse graphs.
- **When to Use:** Small graphs ($V \le 500$) needing all-pairs distances.
- **When NOT to Use:** Large graphs $V > 500$ (cubic time times out).

### Algorithm: 0-1 BFS
- **Input / Output:** Graph with edge weights 0 or 1 $\to$ Shortest distance array.
- **Constraints:** $V, E \le 2 \times 10^5$, edge weights $\in \{0, 1\}$.
- **Brute Force:** Standard Dijkstra in $O(E \log V)$.
- **Optimal Approach:** Deque: push weight 0 edges to **front**, weight 1 edges to **back**.
- **How It Reduces Time/Space:** Maintains monotonic queue ordering without $O(\log V)$ heap overhead.
- **Core Idea:** Deque maintains distance differences bounded by 1.
- **Pattern:** Double-ended priority queue simulation.
- **Data Structure Used:** `ArrayDeque<Integer>`, `int[] dist`.
- **Invariant:** Deque contents span at most two distinct distance values ($d$ and $d + 1$).
- **Dry Run:** 0-weight edges processed immediately at front; 1-weight processed at back.
- **Edge Cases:** Unreachable nodes.
- **Correctness:** Preserves BFS monotonic property in linear time.
- **Time Complexity:** $O(V + E)$ linear time.
- **Space Complexity:** $O(V)$ deque space.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Binary grid movement costs (e.g. 0 to turn, 1 to move).
- **When NOT to Use:** Arbitrary non-binary weights (use Dijkstra).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic shortest path problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Traversal](../traversal/README.md) | [Minimum Spanning Tree](../minimum-spanning-tree/README.md)
