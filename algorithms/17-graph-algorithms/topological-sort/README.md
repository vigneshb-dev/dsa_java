# Topological Sort

> Linear ordering of vertices in a Directed Acyclic Graph (DAG) respecting edge directions.

## 1. Overview
Topological Sort produces a linear ordering of vertices in a Directed Acyclic Graph (DAG) such that for every directed edge `u -> v`, vertex `u` precedes `v` in the ordering. If the graph contains any directed cycle, no topological order exists. Standard algorithms are Kahn's Algorithm (in-degree BFS) and DFS post-order traversal reversal.

## 2. Time & Space Complexity
| Algorithm | Implementation Mechanism | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Kahn's Algorithm | In-degree array + BFS Queue | O(V + E) | O(V) |
| DFS Post-order Reversal | Visited array + Reverse Stack | O(V + E) | O(V) call stack |

If Kahn's algorithm processes fewer than V vertices, the graph contains at least one directed cycle.

## 3. When to Use
- Build system dependency management (e.g., Make, Maven, Gradle task order).
- Course schedule prerequisites and curriculum planning.
- Detecting cycles in directed graphs (Kahn's algorithm).
- Order of evaluation for spreadsheet formulas or compilation symbols.

## 4. When NOT to Use
- Graph is undirected (topological sort requires directed dependency edges).
- Graph is known to contain directed cycles and the cycle nodes must be listed.
- Shortest path on undirected graphs.

## 5. Why It Works
Vertices with in-degree 0 have no outstanding prerequisite dependencies and can be scheduled immediately. Removing a processed vertex and decrementing its neighbors' in-degrees exposes new zero in-degree candidates, peeling the DAG level-by-level.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Permutations) | Check all V! orderings for valid edge directions | O(V! * E) | O(V) |
| Kahn's BFS Algorithm | Queue vertices with in-degree 0 and peel edges | O(V + E) | O(V) |

Tracking in-degrees directly eliminates factorial ordering verification.

## 7. Data Structures Used Here
- `int[] inDegree`: Array tracking unresolved dependencies per node.
- `ArrayDeque<Integer>`: Queue holding zero-in-degree vertices ready for processing.

## 8. Core Template (Java)
```java
// Kahn's Algorithm (BFS Topological Sort)
int[] topologicalSort(int n, List<List<Integer>> adj) {
    int[] inDegree = new int[n];
    for (int u = 0; u < n; u++) {
        for (int v : adj.get(u)) inDegree[v]++;
    }
    Queue<Integer> q = new ArrayDeque<>();
    for (int i = 0; i < n; i++) {
        if (inDegree[i] == 0) q.offer(i);
    }
    int[] order = new int[n];
    int idx = 0;
    while (!q.isEmpty()) {
        int u = q.poll();
        order[idx++] = u;
        for (int v : adj.get(u)) {
            if (--inDegree[v] == 0) q.offer(v);
        }
    }
    return idx == n ? order : new int[0]; // empty if cycle detected
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | [Minimum Spanning Tree](../minimum-spanning-tree/README.md) | [Cycle Detection](../cycle-detection/README.md)
