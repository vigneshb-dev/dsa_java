# Connected Components
> Partitioning graphs into maximal connected subgraphs where every vertex can reach every other.

## 1. Overview
Connected Components algorithms partition the vertices of a graph into maximal subsets such that every pair of vertices within a subset can reach each other. In undirected graphs, components are simple connected sets (found via BFS, DFS, or DSU). In directed graphs, Strongly Connected Components (SCCs) require bidirectional reachability, solved in linear time using Kosaraju's or Tarjan's algorithm.

## 2. Input / Output
- Input: Graph adjacency list with $V$ vertices.
- Output: Total component count, or component IDs array `int[] componentId` mapping each vertex to its partition.

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- Graph can be directed or undirected.

## 4. Brute-Force Approach
- Idea: For every pair $(u, v)$, run BFS to check if $u$ can reach $v$ and $v$ can reach $u$.
- Pseudocode: Pairwise reachability checks.
- Time: $O(V^2 \cdot (V + E)) = O(V^3)$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: In undirected graphs, run a single outer loop calling BFS/DFS to label each component. In directed graphs, Tarjan's algorithm uses DFS traversal with `tin` (discovery time) and `low` links to identify SCC roots in a single pass.
```java
// Reusable Tarjan's Strongly Connected Components (SCC) Template
import java.util.*;

public class ConnectedComponentsTemplate {
    private static int timer;

    public static List<List<Integer>> tarjanSCC(int n, List<List<Integer>> adj) {
        int[] tin = new int[n];
        int[] low = new int[n];
        boolean[] inStack = new boolean[n];
        ArrayDeque<Integer> stack = new ArrayDeque<>();
        List<List<Integer>> sccs = new ArrayList<>();
        Arrays.fill(tin, -1);
        timer = 0;

        for (int i = 0; i < n; i++) {
            if (tin[i] == -1) {
                dfs(i, adj, tin, low, inStack, stack, sccs);
            }
        }
        return sccs;
    }

    private static void dfs(int u, List<List<Integer>> adj, int[] tin, int[] low,
                            boolean[] inStack, ArrayDeque<Integer> stack, List<List<Integer>> sccs) {
        tin[u] = low[u] = timer++;
        stack.push(u);
        inStack[u] = true;

        for (int v : adj.get(u)) {
            if (tin[v] == -1) {
                dfs(v, adj, tin, low, inStack, stack, sccs);
                low[u] = Math.min(low[u], low[v]);
            } else if (inStack[v]) {
                low[u] = Math.min(low[u], tin[v]);
            }
        }

        // If u is root of an SCC, pop all vertices in this SCC
        if (low[u] == tin[u]) {
            List<Integer> component = new ArrayList<>();
            while (true) {
                int node = stack.pop();
                inStack[node] = false;
                component.add(node);
                if (node == u) break;
            }
            sccs.add(component);
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Eliminates pairwise $O(V^2)$ BFS reachability testing.
- **Cases skipped:** Vertices already assigned to an SCC are popped from the stack and never re-evaluated.
- **Shortcuts / tricks used:** Tracking `low[u]` (earliest reachable ancestor still on stack) reveals SCC roots in a single DFS pass.
- **Time saved:** $O(V^3) \to O(V + E)$ linear time.
- **Space effect:** Allocates $O(V)$ auxiliary arrays and stack.
- **Trade-off:** Tarjan's code is structurally intricate.

## 6. Core Idea
In an SCC, every vertex can reach every other. Contracting each SCC into a single super-vertex transforms any directed graph into an acyclic condensation graph (DAG).

## 7. Pattern
- Pattern: DFS Low-Link Value Tracking / Condensation Graph.
- Signals: "Number of provinces / islands", "strongly connected components", "2-SAT satisfiability", "critical connections in network (bridges/articulation points)".

## 8. Data Structure Used
- `int[] tin` (discovery timestamps) and `int[] low` (lowest reachable timestamp).
- Explicit `ArrayDeque<Integer>` stack and `boolean[] inStack`.

## 9. Invariant
A vertex $u$ is the root of an SCC if and only if `low[u] == tin[u]`, meaning none of its descendants can reach any ancestor above $u$.

## 10. Dry Run
Tarjan's on `0 -> 1 -> 2 -> 0` and `2 -> 3`:
| Step | Node | `tin`, `low` | Action | Stack State |
|---|---|---|---|---|
| 1 | 0 | `tin=0, low=0` | Recurse to 1 | `[0]` |
| 2 | 1 | `tin=1, low=1` | Recurse to 2 | `[0, 1]` |
| 3 | 2 | `tin=2, low=2` | Recurse to 3; back-edge to 0 updates `low[2]=0` | `[0, 1, 2]` |
| 4 | 3 | `tin=3, low=3` | Dead end; `low[3]==tin[3]` $\implies$ **Pop SCC `{3}`** | `[0, 1, 2]` |
| 5 | 0 | `low[0]==tin[0]` | Root of SCC $\implies$ **Pop SCC `{2, 1, 0}`** | `[]` |

## 11. Edge Cases
- Disconnected graph: outer loop guarantees all components are found.
- Self-loops: form a valid SCC of size 1.
- Updating `low[u]` using cross-edges not in stack: guarded by `if (inStack[v])`.

## 12. Correctness
Proved by tree-depth properties of DFS: An SCC must form a contiguous subtree in the DFS spanning forest with a unique root node whose `low` value equals its `tin` value.

## 13. Time Complexity
- Best / Average / Worst: $O(V + E)$ linear time.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for timestamps, stack, and visited markers.

## 15. Can It Be Optimized?
Linear $O(V + E)$ time is optimal because every vertex and edge must be inspected.

## 16. When Should I Use This Algorithm?
- Counting connected components or islands in undirected graphs.
- Partitioning directed networks into strongly connected components.
- Solving 2-SAT (Boolean Satisfiability) problems in linear time.
- Condensing cyclic directed graphs into DAGs for dynamic programming.
- Identifying critical bridges and articulation points in networks.

## 17. When Should I NOT Use It?
- Undirected graphs (use simpler BFS or DSU).
- Bipartite verification (use 2-coloring).
- Shortest path queries (use BFS or Dijkstra).

---

## Comparison Table
| Algorithm | Time | Space | Graph Type | Best Use |
|---|---|---|---|---|
| Undirected BFS/DFS | $O(V + E)$ | $O(V)$ | Undirected | Simple component counting, flood fill |
| DSU (Union-Find) | $O(V + E \cdot \alpha(V))$ | $O(V)$ | Undirected | Dynamic/online edge additions |
| Tarjan's SCC | $O(V + E)$ | $O(V)$ | Directed | 1-pass SCC discovery, bridges, 2-SAT |
| Kosaraju's SCC | $O(V + E)$ | $O(V)$ | Directed | 2-pass SCC discovery using transposed graph |

---

### Algorithm: Tarjan's Algorithm (SCC)
- **Input / Output:** Directed graph $\to$ List of strongly connected components.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Pairwise reachability ($O(V^3)$).
- **Optimal Approach:** Single DFS pass tracking `tin` and `low`, popping stack when `low[u] == tin[u]`.
- **How It Reduces Time/Space:** Resolves SCC roots in post-order without graph transposition.
- **Core Idea:** Subtree low-links identify whether a cycle returns above current root.
- **Pattern:** Low-link value DFS.
- **Data Structure Used:** `int[] tin, low`, `ArrayDeque<Integer> stack`, `boolean[] inStack`.
- **Invariant:** Stack preserves vertices belonging to currently open SCC candidates.
- **Dry Run:** Visited nodes enter stack; cycles lower `low` values; roots pop complete SCCs.
- **Edge Cases:** Cross-edges to already closed SCCs skipped via `inStack`.
- **Correctness:** DFS subtree decomposition.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Production and competitive programming default for SCCs and bridges.
- **When NOT to Use:** Undirected graphs (use standard DFS).

### Algorithm: Kosaraju's Algorithm (SCC)
- **Input / Output:** Directed graph $\to$ List of strongly connected components.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Pairwise reachability ($O(V^3)$).
- **Optimal Approach:** Pass 1: Order vertices by DFS finish time. Pass 2: Run DFS on transposed graph in reverse finish order.
- **How It Reduces Time/Space:** Graph transposition flips edge directions, trapping DFS within individual SCCs.
- **Core Idea:** Reversing edges turns source SCCs in DAG condensation into sink SCCs.
- **Pattern:** Two-pass DFS on transposed graph.
- **Data Structure Used:** `ArrayDeque<Integer>` stack, transposed adjacency list `List<List<Integer>>`.
- **Invariant:** In transposed graph, vertices in the same SCC remain mutually reachable.
- **Dry Run:** DFS fills stack by finish times; pops from stack to run second DFS collecting SCCs.
- **Edge Cases:** Disconnected components.
- **Correctness:** Condensation DAG topological property.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V + E)$ to store transposed graph.
- **Can It Be Optimized:** Tarjan's does it in 1 pass without building transposed graph.
- **When to Use:** Pedagogically simpler concept than Tarjan's.
- **When NOT to Use:** When building transposed graph is memory-prohibitive.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic connected components problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Cycle Detection](../cycle-detection/README.md) | [Bipartite Graph](../bipartite/README.md)
