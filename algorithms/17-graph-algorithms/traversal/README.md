# Graph Traversal

> BFS for shortest unweighted paths and level exploration; DFS for deep connectivity and paths.

## 1. Overview
Graph Traversal systematically visits all vertices and edges in a graph. Breadth-First Search (BFS) explores nodes level-by-level using a queue, guaranteeing the shortest path in unweighted graphs. Depth-First Search (DFS) explores as deep as possible along each branch before backtracking, making it ideal for pathfinding, topological ordering, and cycle detection.

## 2. Time & Space Complexity
| Traversal Algorithm | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| Breadth-First Search (BFS) | O(V + E) | O(V) queue and visited set |
| Depth-First Search (DFS - Recursive) | O(V + E) | O(V) call stack and visited set |
| Depth-First Search (DFS - Iterative) | O(V + E) | O(V) explicit stack |

A `boolean[] visited` array prevents visiting nodes multiple times and protects against infinite loops in cyclic graphs.

## 3. When to Use
- BFS: Shortest path in unweighted graphs or uniform step mazes.
- BFS: Level-order traversal or expanding concentric wavefronts.
- DFS: Finding any path between source and target.
- DFS: Connected component identification and flood filling.

## 4. When NOT to Use
- Edges have varying non-uniform weights (use Dijkstra or Bellman-Ford).
- Call stack depth may exceed JVM recursion limits for DFS (use iterative DFS with `ArrayDeque`).
- Graph is bipartite and requires max-flow algorithms.

## 5. Why It Works
BFS pushes adjacent unvisited nodes to the rear of a FIFO queue, ensuring distance `d` nodes depart before distance `d + 1` nodes. DFS uses LIFO stack ordering to pursue a path until exhaustion before retreating to previous branch points.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Unchecked Exploration | Traverse edges without tracking visited nodes | Infinite (cycles) | O(infinity) |
| Visited Array Guard | Mark vertices visited immediately upon discovery | O(V + E) | O(V) |

The visited array prevents infinite loops in cycles and ensures each edge is examined at most twice.

## 7. Data Structures Used Here
- `ArrayDeque<Integer>`: Queue for BFS, explicit stack for iterative DFS.
- `boolean[] visited`: Visited set tracking.

## 8. Core Template (Java)
```java
// BFS Shortest Path in Unweighted Graph
int bfsShortestPath(int start, int target, List<List<Integer>> adj, int n) {
    Queue<Integer> q = new ArrayDeque<>();
    boolean[] visited = new boolean[n];
    q.offer(start);
    visited[start] = true;
    int steps = 0;
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            int u = q.poll();
            if (u == target) return steps;
            for (int v : adj.get(u)) {
                if (!visited[v]) {
                    visited[v] = true;
                    q.offer(v);
                }
            }
        }
        steps++;
    }
    return -1;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Medium | Example template placeholder |

## 10. Navigation
[Main README](../../../README.md) | [Progress](../../../PROGRESS.md) | [Graph Algorithms](../README.md) | Start | [Shortest Path](../shortest-path/README.md)
