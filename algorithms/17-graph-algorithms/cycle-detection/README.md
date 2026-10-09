# Cycle Detection
> Identifying cycles and feedback loops in directed and undirected graphs in linear time.

## 1. Overview
Cycle detection determines whether a graph contains at least one circular path of edges starting and ending at the same vertex. The approach differs fundamentally based on graph directionality: undirected graphs use Disjoint Set Union (Union-Find) or simple BFS/DFS visited tracking with a parent pointer, whereas directed graphs require 3-color DFS state tracking (detecting back-edges) or Kahn's in-degree algorithm.

## 2. Input / Output
- Input: Graph (directed or undirected) with $V$ vertices and $E$ edges.
- Output: Boolean `true` if a cycle exists, `false` otherwise (or the edge forming the cycle).

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- In undirected graphs without self-loops, if $E \ge V$, a cycle is mathematically guaranteed by the Pigeonhole Principle.

## 4. Brute-Force Approach
- Idea: For every node $u$, run a DFS search to see if a path leads back to $u$ without reusing edges.
- Pseudocode: Run $V$ individual DFS searches.
- Time: $O(V \cdot (V + E)) = O(V^2)$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: For directed graphs, maintain three colors for each node: 0 (Unvisited), 1 (Visiting / On current recursion stack), 2 (Visited / Fully processed). An edge to a node in state 1 is a back-edge, proving a directed cycle. For undirected graphs, use DSU: if `find(u) == find(v)`, adding edge $(u, v)$ creates a cycle.
```java
// Reusable Cycle Detection Template (3-Color DFS for Directed Graphs)
import java.util.*;

public class CycleDetectionTemplate {
    // 0 = Unvisited, 1 = Visiting (in current recursion path), 2 = Visited (finished)
    public static boolean hasCycleDirected(int n, List<List<Integer>> adj) {
        int[] color = new int[n];
        for (int i = 0; i < n; i++) {
            if (color[i] == 0) {
                if (dfsCheckDirected(i, adj, color)) return true;
            }
        }
        return false;
    }

    private static boolean dfsCheckDirected(int u, List<List<Integer>> adj, int[] color) {
        color[u] = 1; // Mark visiting

        for (int v : adj.get(u)) {
            if (color[v] == 1) {
                return true; // Back-edge detected! Cycle confirmed
            }
            if (color[v] == 0 && dfsCheckDirected(v, adj, color)) {
                return true;
            }
        }

        color[u] = 2; // Mark fully processed
        return false;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the need to restart a separate DFS from every vertex.
- **Cases skipped:** Vertices marked color 2 (fully processed) are known to lead to no cycles and are skipped immediately.
- **Shortcuts / tricks used:** Three-state flag distinguishes between cross-edges/forward-edges and true ancestral back-edges.
- **Time saved:** $O(V^2) \to O(V + E)$ linear single pass.
- **Space effect:** Allocates $O(V)$ color array and call stack.
- **Trade-off:** Must strictly separate directed cycle logic from undirected cycle logic.

## 6. Core Idea
A directed cycle exists if and only if a DFS traversal encounters a back-edge pointing to an ancestor currently residing on the active recursion call stack.

## 7. Pattern
- Pattern: 3-Color DFS State Machine / DSU Disjoint Set Merging.
- Signals: "Course Schedule", "redundant connection", "detect deadlock in operating system", "is graph a valid tree".

## 8. Data Structure Used
- Directed: `int[] color` array (states 0, 1, 2) or `boolean[] inStack`.
- Undirected: DSU (`parent[]`) or `boolean[] visited` with `parent` integer.

## 9. Invariant
A node has color 1 if and only if it is an ancestor of the currently active DFS node in the recursion tree.

## 10. Dry Run
Directed cycle detection on `0 -> 1 -> 2 -> 0`:
| Step | Active Node $u$ | Color State | Action | Next State |
|---|---|---|---|---|
| 1 | Node 0 | `color[0] = 1` | Recurse to 1 | - |
| 2 | Node 1 | `color[1] = 1` | Recurse to 2 | - |
| 3 | Node 2 | `color[2] = 1` | Inspect neighbor 0 | `color[0] == 1` $\implies$ **Cycle Detected!** |

## 11. Edge Cases
- Self-loops ($u \to u$): immediately detected because `color[u] == 1`.
- Undirected graph with trivial back-edge to parent: must check `if (v != parent)` to avoid false positive cycle detections.
- Disconnected graph with cycles in secondary components: outer loop over all nodes ensures every component is checked.

## 12. Correctness
By White-Path Theorem and DFS edge classification: In any DFS forest, directed edges are classified into tree, forward, back, or cross edges. A directed graph has a cycle if and only if the DFS forest contains at least one back edge (an edge pointing to an ancestor whose color is 1).

## 13. Time Complexity
- Best / Average / Worst: $O(V + E)$ linear time single pass.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for color/visited array and recursion call stack.

## 15. Can It Be Optimized?
Linear $O(V + E)$ time is optimal since every edge may need to be inspected.

## 16. When Should I Use This Algorithm?
- Deadlock detection in multi-threaded lock allocation graphs.
- Detecting circular dependencies in build configurations or spreadsheets.
- Validating whether a graph is a valid tree ($E == V - 1$ and acyclic).
- Finding redundant connections in network cabling.

## 17. When Should I NOT Use It?
- Directed graph already requiring full topological sort (use Kahn's algorithm directly; it detects cycles as a natural side effect).
- Finding shortest cycle length / girth in unweighted graph (use BFS from every node in $O(V(V + E))$).

---

## Comparison Table
| Algorithm | Time | Space | Graph Type | Best Use |
|---|---|---|---|---|
| 3-Color DFS | $O(V + E)$ | $O(V)$ | Directed | Directed graphs, recursive dependency checks |
| Kahn's In-Degree BFS | $O(V + E)$ | $O(V)$ | Directed | Directed graphs, queue-based without recursion |
| DSU (Union-Find) | $O(E \cdot \alpha(V))$ | $O(V)$ | Undirected | Undirected graphs, online stream of edges |
| DFS with Parent Pointer | $O(V + E)$ | $O(V)$ | Undirected | Undirected graphs with adjacency list |

---

### Algorithm: 3-Color DFS Cycle Detection (Directed)
- **Input / Output:** Directed graph $\to$ Boolean cycle flag.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Path search ($O(V^2)$).
- **Optimal Approach:** DFS with states: 0=unvisited, 1=visiting, 2=finished.
- **How It Reduces Time/Space:** Distinguishes between ancestors (color 1) and cross-nodes (color 2).
- **Core Idea:** Hitting a node of color 1 detects an ancestral back-edge (loop).
- **Pattern:** 3-state coloring DFS.
- **Data Structure Used:** `int[] color`.
- **Invariant:** Nodes of color 1 currently lie on the active recursion call stack.
- **Dry Run:** Dives along path; if neighbor is color 1, returns true.
- **Edge Cases:** Multiple components; self-loops.
- **Correctness:** By White-Path Theorem; back-edges exist iff cycles exist.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Standard cycle check in directed graphs.
- **When NOT to Use:** Undirected graphs (use DSU).

### Algorithm: DSU Cycle Detection (Undirected)
- **Input / Output:** Undirected edge list $\to$ Boolean cycle flag or redundant edge.
- **Constraints:** $V, E \le 2 \times 10^5$, undirected edges.
- **Brute Force:** Full DFS per edge ($O(E \cdot V)$).
- **Optimal Approach:** Iterate edges; if `find(u) == find(v)` return true; else `union(u, v)`.
- **How It Reduces Time/Space:** Near constant $O(\alpha(V))$ check per edge.
- **Core Idea:** Adding an edge between two vertices already in the same component forms a cycle.
- **Pattern:** Disjoint Set Union.
- **Data Structure Used:** `int[] parent`.
- **Invariant:** Each tree in DSU forest represents a connected component.
- **Dry Run:** Merges sets; when edge targets same set, cycle detected.
- **Edge Cases:** Self-loops, disconnected components.
- **Correctness:** Spanning forest properties in undirected graphs.
- **Time Complexity:** $O(E \cdot \alpha(V))$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Online streaming undirected edge processing; Redundant Connection I.
- **When NOT to Use:** Directed graphs (DSU does not distinguish edge orientation).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic cycle detection problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Topological Sort](../topological-sort/README.md) | [Connected Components](../connected-components/README.md)
