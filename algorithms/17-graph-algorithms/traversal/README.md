# Graph Traversal
> Systematic exploration of graph vertices and edges via Breadth-First Search and Depth-First Search.

## 1. Overview
Graph Traversal algorithms visit every vertex and edge reachable from a source vertex in a graph without entering infinite loops on cycles. Breadth-First Search (BFS) explores radially level-by-level, while Depth-First Search (DFS) explores along each branch as deeply as possible before backtracking.

## 2. Input / Output
- Input: Graph adjacency list and source vertex $s$ (e.g. `adj` with edges `0-1, 0-2, 1-3`, $s = 0$).
- Output: Traversal visitation order or reachability array (e.g. BFS: `[0, 1, 2, 3]`; DFS: `[0, 1, 3, 2]`).

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- Graphs can be directed or undirected, disconnected, or cyclic.

## 4. Brute-Force Approach
- Idea: Randomly follow edges without tracking visited state.
- Pseudocode: Unchecked recursion causing infinite cycle loops.
- Time: Infinite loops on cycles; Space: Stack overflow.

## 5. Optimal Approach
- Idea: Maintain a `boolean[] visited` array. Use a FIFO `Queue` for BFS (level-by-level) or a recursion/stack for DFS (deep branching).
```java
// Reusable Graph Traversal Skeleton (BFS and DFS)
import java.util.*;

public class TraversalTemplate {
    public static void bfs(List<List<Integer>> adj, int start, boolean[] visited) {
        ArrayDeque<Integer> queue = new ArrayDeque<>();
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

    public static void dfs(List<List<Integer>> adj, int u, boolean[] visited) {
        visited[u] = true;
        for (int v : adj.get(u)) {
            if (!visited[v]) {
                dfs(adj, v, visited);
            }
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Prevents traversing edges into already-visited vertices.
- **Cases skipped:** Cycles and back-edges are detected in $O(1)$ by the `visited` flag and skipped immediately.
- **Shortcuts / tricks used:** Marking vertices visited upon **enqueue** (in BFS) prevents duplicate queue insertions.
- **Time saved:** From infinite loops / exponential paths to strict linear $O(V + E)$ time.
- **Space effect:** Allocates $O(V)$ auxiliary memory for the visited array and queue/stack.
- **Trade-off:** Requires keeping state of all vertices.

## 6. Core Idea
Every vertex is visited at most once by marking visited flags upon initial discovery, ensuring the search forms a spanning forest across the graph.

## 7. Pattern
- Pattern: Level-Order Exploration (BFS) / Deep Branch Backtracking (DFS).
- Signals: "Find if path exists", "flood fill / number of islands", "clone graph", "word ladder", "maze exploration".

## 8. Data Structure Used
- `ArrayDeque` for BFS queue; call stack or explicit stack for DFS.
- `boolean[] visited` array of size $V$.

## 9. Invariant
A vertex marked `visited[u] == true` will never be added to the work queue or call stack a second time.

## 10. Dry Run
BFS on `0-1, 0-2, 1-2` from 0:
| Step | Queue | Polled | Neighbors Checked | Visited Array |
|---|---|---|---|---|
| Init | `[0]` | - | - | `{0:T, 1:F, 2:F}` |
| 1 | `[1, 2]` | 0 | 1 (mark T), 2 (mark T) | `{0:T, 1:T, 2:T}` |
| 2 | `[2]` | 1 | 2 (already visited) | `{0:T, 1:T, 2:T}` |
| 3 | `[]` | 2 | None unvisited | `{0:T, 1:T, 2:T}` |

## 11. Edge Cases
- Disconnected graph: outer loop over all vertices $i = 0 \dots V-1$ to restart traversal on unvisited vertices.
- Marking visited on dequeue instead of enqueue in BFS: causes massive memory explosion from duplicate enqueues.
- Deep graphs causing recursion stack overflow in DFS: convert to iterative DFS using `ArrayDeque`.

## 12. Correctness
By induction: Every vertex reachable in 0 steps (source) is visited. If all vertices at distance $\le k$ are visited, any unvisited neighbor at distance $k + 1$ is discovered and visited, guaranteeing full reachability exploration.

## 13. Time Complexity
- Best / Average / Worst: $O(V + E)$ linear time. Every vertex is visited once, and every edge is examined once (directed) or twice (undirected).

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for queue / recursion stack and visited array.

## 15. Can It Be Optimized?
Linear $O(V + E)$ time is already optimal. Bidirectional BFS reduces memory and search space for point-to-point queries.

## 16. When Should I Use This Algorithm?
- Checking if a path exists between two nodes.
- Finding shortest path in unweighted graphs (BFS).
- Connected component discovery and flood fill (2D grids).
- Topological sorting and cycle detection (DFS).
- Bipartite graph verification.

## 17. When Should I NOT Use It?
- Weighted graphs seeking shortest paths (use Dijkstra or Bellman-Ford).
- Minimum spanning trees (use Kruskal or Prim).
- Massive graphs where target is near start in all directions (use A* Search).

---

## Comparison Table
| Algorithm | Time | Space | Stable/Notes | Best Use |
|---|---|---|---|---|
| Breadth-First Search (BFS) | $O(V + E)$ | $O(V)$ | Level-by-level (FIFO queue) | Shortest path in unweighted graphs |
| Depth-First Search (DFS) | $O(V + E)$ | $O(V)$ | Branch-deep (LIFO stack / recursion) | Backtracking, cycle detection, topological sort |

---

### Algorithm: Breadth-First Search (BFS)
- **Input / Output:** Graph and source $s$ $\to$ Level-order traversal or shortest distance array.
- **Constraints:** $V, E \le 2 \times 10^5$, unweighted edges.
- **Brute Force:** DFS search exploring long paths first ($O(V + E)$ but does not guarantee shortest path in one pass).
- **Optimal Approach:** Use FIFO queue; mark visited upon enqueue.
- **How It Reduces Time/Space:** Explores vertices in increasing order of distance from source.
- **Core Idea:** Concentric waves expanding outward from source.
- **Pattern:** Level-order frontier exploration.
- **Data Structure Used:** `ArrayDeque` (FIFO queue) and `boolean[] visited`.
- **Invariant:** When node at distance $d$ is dequeued, all nodes at distance $< d$ have been processed.
- **Dry Run:** Source 0 enqueues direct neighbors, then neighbors' neighbors.
- **Edge Cases:** Disconnected graphs, high branching factor memory peaks.
- **Correctness:** Follows from FIFO queue ordering guaranteeing monotonic non-decreasing path lengths.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$ queue width.
- **Can It Be Optimized:** Bidirectional BFS reduces search volume from $b^d$ to $2b^{d/2}$.
- **When to Use:** Shortest path in unweighted graphs; word ladders; level-order traversal.
- **When NOT to Use:** Weighted graphs (use Dijkstra) or exhaustive tree backtracking (use DFS).

### Algorithm: Depth-First Search (DFS)
- **Input / Output:** Graph and source $s$ $\to$ Subtree components, entry/exit timestamps, cycle flags.
- **Constraints:** $V, E \le 2 \times 10^5$, arbitrary topology.
- **Brute Force:** Path enumeration without visited tracking ($O(V!)$).
- **Optimal Approach:** Recursive function marking visited on entry.
- **How It Reduces Time/Space:** Backtracks immediately upon hitting visited nodes or dead ends.
- **Core Idea:** Plunge as deep as possible down a branch before backtracking.
- **Pattern:** Recursive branch exploration.
- **Data Structure Used:** JVM call stack or explicit stack.
- **Invariant:** Current path forms a simple path from root to active vertex in DFS tree.
- **Dry Run:** Visits $0 \to 1 \to 3$ until leaf, backtracks to 1, visits alternative child.
- **Edge Cases:** Deep trees triggering `StackOverflowError` (use iterative DFS).
- **Correctness:** Induction on branch exploration.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$ call-stack depth.
- **Can It Be Optimized:** Iterative stack avoids call stack limits.
- **When to Use:** Cycle detection, connected components, path finding, maze solving.
- **When NOT to Use:** Shortest path in unweighted graphs (use BFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic traversal problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Overview](../README.md) | [Shortest Path](../shortest-path/README.md)
