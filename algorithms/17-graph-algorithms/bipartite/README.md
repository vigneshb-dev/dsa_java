# Bipartite Graph
> Verifying 2-colorability and odd-cycle absence in graphs using alternating vertex coloring.

## 1. Overview
A Bipartite Graph is a graph whose vertices can be partitioned into two independent sets $U$ and $V$ such that every edge connects a vertex in $U$ to a vertex in $V$ (no internal edges within either set). By König's theorem, a graph is bipartite if and only if it contains no odd-length cycles, which can be verified in linear $O(V + E)$ time using 2-Coloring BFS or DFS.

## 2. Input / Output
- Input: Graph adjacency list with $V$ vertices.
- Output: Boolean `true` if graph is bipartite, `false` otherwise.

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- Graph can be disconnected; self-loops immediately make a graph non-bipartite.

## 4. Brute-Force Approach
- Idea: Enumerate all $2^V$ possible partitions into two sets and check whether any edge has endpoints in the same set.
- Pseudocode: Test all binary partition assignments.
- Time: $O(2^V \cdot E)$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: Assign color 1 to starting node. For every visited node with color $c$, assign color $1 - c$ to all its neighbors. If an adjacent neighbor already possesses color $c$, a monochromatic edge exists, proving the graph contains an odd cycle and is not bipartite.
```java
// Reusable Bipartite 2-Coloring BFS Template
import java.util.*;

public class BipartiteTemplate {
    public static boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1); // -1 = uncolored, 0 = color A, 1 = color B

        for (int i = 0; i < n; i++) {
            if (color[i] != -1) continue; // Skip already colored components

            ArrayDeque<Integer> queue = new ArrayDeque<>();
            color[i] = 0;
            queue.offer(i);

            while (!queue.isEmpty()) {
                int u = queue.poll();

                for (int v : graph[u]) {
                    if (color[v] == -1) {
                        color[v] = 1 - color[u]; // Color neighbor with opposite color
                        queue.offer(v);
                    } else if (color[v] == color[u]) {
                        return false; // Same color on both ends: odd cycle detected!
                    }
                }
            }
        }

        return true;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the exponential exploration of $2^V$ set partitions.
- **Cases skipped:** If any conflict is detected, the algorithm halts immediately and returns `false`.
- **Shortcuts / tricks used:** Deterministic alternating assignment ($color[v] = 1 - color[u]$) eliminates ambiguity.
- **Time saved:** $O(2^V \cdot E) \to O(V + E)$ linear time.
- **Space effect:** Allocates $O(V)$ auxiliary memory for the color array and queue.
- **Trade-off:** Minimal memory allocation.

## 6. Core Idea
A graph can be 2-colored if and only if it does not contain any odd cycles. Alternating colors along BFS/DFS levels reveals any odd cycle as an edge between two vertices with the same color.

## 7. Pattern
- Pattern: 2-Coloring Graph Verification / Odd-Cycle Detection.
- Signals: "Divide people into two groups with no dislike pairs", "possible bipartition", "is graph bipartite", "maximum bipartite matching prerequisite".

## 8. Data Structure Used
- `int[] color` array initialized to -1.
- `ArrayDeque<Integer>` FIFO queue (or call stack for DFS).

## 9. Invariant
All processed edges connect vertices with strictly different colors ($color[u] \ne color[v]$).

## 10. Dry Run
Checking triangle `0-1, 1-2, 0-2`:
| Step | Node | Assigned Color | Neighbors Inspected | Neighbor Colors | Conflict? |
|---|---|---|---|---|---|
| 1 | 0 | 0 | 1 | `color[1] = 1` | None |
| 2 | 1 | 1 | 2 | `color[2] = 0` | None |
| 3 | 2 | 0 | 0 | `color[0] = 0` | **Conflict!** ($color[2] == color[0]$) $\implies$ Returns `false` |

## 11. Edge Cases
- Disconnected graph: outer loop guarantees all components are verified.
- Graph with no edges (isolated vertices): bipartite by definition (returns `true`).
- Self-loops: immediately non-bipartite ($color[u] == color[u]$).

## 12. Correctness
By König's theorem (1936): A graph is bipartite iff it has no odd cycles. If BFS encounters an edge between two vertices of the same color, their paths to the common tree ancestor must have equal length, forming an odd cycle with the cross-edge.

## 13. Time Complexity
- Best: $O(1)$ on early conflict.
- Average / Worst: $O(V + E)$ linear time.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for color array and queue/stack.

## 15. Can It Be Optimized?
Linear $O(V + E)$ time and $O(V)$ space are optimal.

## 16. When Should I Use This Algorithm?
- Checking if a graph can be divided into two independent sets.
- Verifying whether a graph contains any odd cycles.
- Prerequisite check before Maximum Bipartite Matching (Hopcroft-Karp).
- Two-party conflict resolution (e.g. Possible Bipartition).

## 17. When Should I NOT Use It?
- Testing for 3 or more colors (3-Coloring is NP-complete).
- Directed graphs (concept applies strictly to undirected graphs).
- Tree graphs (all trees are bipartite by definition, no check required).

---

## Comparison Table
| Algorithm | Time | Space | Traversal Type | Best Use |
|---|---|---|---|---|
| 2-Coloring BFS | $O(V + E)$ | $O(V)$ | Queue-based BFS | Default standard; avoids recursion stack overflow |
| 2-Coloring DFS | $O(V + E)$ | $O(V)$ | Call-stack DFS | Concise recursive code for shallow graphs |

---

### Algorithm: 2-Coloring via BFS
- **Input / Output:** Graph adjacency list $\to$ Boolean bipartite status.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Partition testing ($O(2^V)$).
- **Optimal Approach:** BFS queue alternating colors $1 - c$.
- **How It Reduces Time/Space:** Linear radial coloring.
- **Core Idea:** Level-by-level alternating color assignment.
- **Pattern:** BFS with 2-color state.
- **Data Structure Used:** `ArrayDeque<Integer>`, `int[] color`.
- **Invariant:** Adjacent nodes across BFS edges have opposite colors.
- **Dry Run:** Enqueues node, colors neighbors with $1 - c$, checks conflicts.
- **Edge Cases:** Disconnected graphs handled by outer loop.
- **Correctness:** Follows from König's theorem.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Standard production implementation.
- **When NOT to Use:** When DFS recursion is preferred for brevity on small graphs.

### Algorithm: 2-Coloring via DFS
- **Input / Output:** Graph adjacency list $\to$ Boolean bipartite status.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Partition testing ($O(2^V)$).
- **Optimal Approach:** Recursive DFS returning false if neighbor color matches current.
- **How It Reduces Time/Space:** Early return unwinds call stack immediately on conflict.
- **Core Idea:** Alternating coloring down recursive branch.
- **Pattern:** DFS with 2-color state.
- **Data Structure Used:** Call stack, `int[] color`.
- **Invariant:** Active DFS path alternates colors monotonically.
- **Dry Run:** Dives into neighbor; if colored same, returns false.
- **Edge Cases:** Deep graphs causing recursion stack overflow.
- **Correctness:** Odd-cycle back-edge discovery.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$ call stack.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Quick recursive implementation.
- **When NOT to Use:** Deep graphs with $V > 10^4$ (use BFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic bipartite graph problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Connected Components](../connected-components/README.md) | [Graph Overview](../README.md)
