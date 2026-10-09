# Topological Sort
> Linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge (u, v), u precedes v.

## 1. Overview
Topological Sort establishes a valid sequential execution order for vertices in a Directed Acyclic Graph (DAG) respecting all prerequisite dependencies. The two canonical implementations are Kahn's Algorithm (in-degree reduction using BFS) and DFS Post-Order (reversing completion timestamps), both running in linear $O(V + E)$ time and simultaneously detecting directed cycles.

## 2. Input / Output
- Input: Directed graph with $V$ vertices and dependency edges (e.g. `0 -> 1, 0 -> 2, 1 -> 3, 2 -> 3`).
- Output: A valid topological sequence array or empty array if a cycle exists (e.g. `[0, 1, 2, 3]` or `[0, 2, 1, 3]`).

## 3. Constraints
- Vertices $V \le 10^5$, Edges $E \le 2 \times 10^5$.
- Graph MUST be directed. If graph contains any directed cycle, no topological ordering exists.

## 4. Brute-Force Approach
- Idea: Enumerate all $V!$ permutations of vertices and verify whether all directed edges point from left to right.
- Pseudocode: Permutation generation followed by $O(E)$ validation.
- Time: $O(V! \cdot E)$; Space: $O(V)$.

## 5. Optimal Approach
- Idea: Compute in-degrees for all vertices. Enqueue all vertices with in-degree 0. Repeatedly poll a vertex, append to output, and decrement in-degrees of its outgoing neighbors, enqueuing any neighbor whose in-degree reaches 0.
```java
// Reusable Topological Sort Skeleton (Kahn's BFS Algorithm Template)
import java.util.*;

public class TopologicalSortTemplate {
    public static int[] kahnTopologicalSort(int n, List<List<Integer>> adj) {
        int[] inDegree = new int[n];
        for (int u = 0; u < n; u++) {
            for (int v : adj.get(u)) inDegree[v]++;
        }

        ArrayDeque<Integer> queue = new ArrayDeque<>();
        for (int i = 0; i < n; i++) {
            if (inDegree[i] == 0) queue.offer(i); // Free of dependencies
        }

        int[] order = new int[n];
        int index = 0;

        while (!queue.isEmpty()) {
            int u = queue.poll();
            order[index++] = u;

            for (int v : adj.get(u)) {
                inDegree[v]--;
                if (inDegree[v] == 0) {
                    queue.offer(v); // All prerequisites for v are satisfied
                }
            }
        }

        // If not all vertices are processed, a directed cycle exists
        return (index == n) ? order : new int[0];
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes the exponential validation of invalid vertex orderings.
- **Cases skipped:** Vertices with outstanding unfulfilled prerequisites are never enqueued prematurely.
- **Shortcuts / tricks used:** Tracking in-degree integer counts allows constant-time $O(1)$ discovery of newly unlocked tasks.
- **Time saved:** $O(V! \cdot E) \to O(V + E)$ linear time.
- **Space effect:** Allocates $O(V)$ auxiliary in-degree array and queue.
- **Trade-off:** Graph must be directed and acyclic.

## 6. Core Idea
A DAG always contains at least one vertex with in-degree 0 (a source). Processing sources first and stripping their outgoing edges reveals new sources iteratively until all dependencies are satisfied.

## 7. Pattern
- Pattern: In-Degree Reduction (Kahn's BFS) / Reverse DFS Post-Order.
- Signals: "Course Schedule I & II", "build system dependency graph", "alien dictionary", "alien alphabet order", "sequence reconstruction".

## 8. Data Structure Used
- `int[] inDegree` array.
- `ArrayDeque<Integer>` FIFO queue (or `PriorityQueue` for lexicographically smallest order).

## 9. Invariant
At every step in Kahn's algorithm, any vertex entered into the queue has all its incoming prerequisite edges completely fulfilled by previously recorded vertices.

## 10. Dry Run
Kahn's on `0 -> 1, 0 -> 2, 1 -> 3, 2 -> 3`:
| Step | `inDegree` Array | Queue | Polled Node | Output Order |
|---|---|---|---|---|
| Init | `[0, 1, 1, 2]` | `[0]` | - | `[]` |
| 1 | `[0, 0, 0, 2]` | `[1, 2]` | 0 | `[0]` |
| 2 | `[0, 0, 0, 1]` | `[2]` | 1 | `[0, 1]` |
| 3 | `[0, 0, 0, 0]` | `[3]` | 2 | `[0, 1, 2]` |
| 4 | `[0, 0, 0, 0]` | `[]` | 3 | `[0, 1, 2, 3]` |

Count processed $= 4 == V \implies$ valid ordering!

## 11. Edge Cases
- Cycle exists: queue becomes empty before processing all $V$ vertices; handled by checking `index == n`.
- Disconnected components: all isolated nodes start with in-degree 0 and enter the queue immediately.
- Multiple valid orderings: topological order is generally non-unique.

## 12. Correctness
By induction on DAG reduction: Every finite DAG has at least one vertex of in-degree 0. Removing an in-degree 0 vertex and its outgoing edges leaves a smaller DAG. By induction, Kahn's algorithm produces a sequence where each vertex appears only after all its predecessors have been removed.

## 13. Time Complexity
- Best / Average / Worst: $O(V + E)$ linear time. Computes in-degrees in $O(V + E)$ and processes each vertex and edge once.

## 14. Space Complexity
- Auxiliary Space: $O(V)$ for in-degree array and queue.

## 15. Can It Be Optimized?
Linear $O(V + E)$ time is optimal because every vertex and edge must be inspected at least once.

## 16. When Should I Use This Algorithm?
- Determining compilation order of dependent source files or packages.
- Course schedule prerequisite planning.
- Resolving symbol dependencies in software linkers.
- Deriving character ordering from a sorted foreign alphabet (Alien Dictionary).
- Finding longest or shortest paths in a DAG in $O(V + E)$ time.

## 17. When Should I NOT Use It?
- Undirected graphs (dependencies have no directionality; use connected components).
- Graphs with known cycles seeking paths (use cycle detection or feedback arc set).
- Finding shortest path with arbitrary weighted non-DAG graphs (use Dijkstra).

---

## Comparison Table
| Algorithm | Time | Space | Cycle Detection | Best Use |
|---|---|---|---|---|
| Kahn's Algorithm | $O(V + E)$ | $O(V)$ | Yes (`count < V`) | Natural BFS queue; easy cycle check; allows lexicographical sort with PriorityQueue |
| DFS Post-Order | $O(V + E)$ | $O(V)$ | Requires 3-state coloring | Recursive call stack; topological DAG DP |

---

### Algorithm: Kahn's Algorithm (BFS)
- **Input / Output:** Directed graph $\to$ Array of ordered vertices or empty on cycle.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Permutation checking ($O(V!)$).
- **Optimal Approach:** Track in-degrees, enqueue in-degree 0, decrement neighbor in-degrees on poll.
- **How It Reduces Time/Space:** Operates greedily on vertices with zero remaining incoming edges.
- **Core Idea:** Strip zero in-degree nodes iteratively.
- **Pattern:** BFS with in-degree array.
- **Data Structure Used:** `int[] inDegree`, `ArrayDeque<Integer>`.
- **Invariant:** Enqueued vertices have 0 remaining prerequisites.
- **Dry Run:** Enqueues node 0, decreases neighbors, enqueues newly freed nodes.
- **Edge Cases:** Cycles detected when total processed count $< V$.
- **Correctness:** Inductive DAG node elimination.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Standard topological sorting; detecting directed cycles.
- **When NOT to Use:** Undirected graphs.

### Algorithm: DFS Post-Order
- **Input / Output:** Directed graph $\to$ Reversed list of visited vertices.
- **Constraints:** $V, E \le 2 \times 10^5$.
- **Brute Force:** Permutation check ($O(V!)$).
- **Optimal Approach:** DFS traversal; prepend node to linked list or push to stack upon **exiting** DFS function.
- **How It Reduces Time/Space:** Post-order ensures all descendants of $u$ are fully processed before $u$ itself is pushed.
- **Core Idea:** Last node to finish DFS has no outgoing dependencies to unvisited nodes.
- **Pattern:** Post-order DFS with reverse collection.
- **Data Structure Used:** Call stack, `List<Integer>` or stack.
- **Invariant:** When node $u$ finishes DFS, all vertices reachable from $u$ are already placed after $u$.
- **Dry Run:** Recursively dives to leaf nodes; leaves pushed first, root pushed last.
- **Edge Cases:** Cycles require 3-color visited array (`0=unvisited, 1=visiting, 2=visited`) to detect back-edges.
- **Correctness:** Depth-first tree property: if edge $(u, v)$ exists, $v$ finishes before $u$.
- **Time Complexity:** $O(V + E)$.
- **Space Complexity:** $O(V)$ call-stack depth.
- **Can It Be Optimized:** Already optimal.
- **When to Use:** Integrated directly within DFS-based DAG algorithms or DP.
- **When NOT to Use:** Deep graphs risking stack overflow (prefer Kahn's BFS).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic topological sort problem placeholder |

## 19. Navigation
Links: [Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Minimum Spanning Tree](../minimum-spanning-tree/README.md) | [Cycle Detection](../cycle-detection/README.md)
