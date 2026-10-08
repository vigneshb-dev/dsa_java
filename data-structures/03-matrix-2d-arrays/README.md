# Matrix & 2D Arrays

> Grid and table structures modeled as arrays of arrays for 2D spatial problems.

## 1. Overview
A 2D array or matrix represents data in rows and columns forming a rectangular coordinate grid. In Java, multi-dimensional arrays are represented as 'arrays of arrays', meaning each row is an independent array object referenced by the outer array. Common algorithms traverse matrices using coordinate deltas or treat them as implicit graphs.

## 2. Time & Space Complexity
| Operation / Variant | Time (best / average / worst) | Space |
| :--- | :--- | :--- |
| Access Cell `grid[r][c]` | O(1) / O(1) / O(1) | O(1) |
| Update Cell `grid[r][c]` | O(1) / O(1) / O(1) | O(1) |
| Full Traversal (m x n) | O(m*n) / O(m*n) / O(m*n) | O(1) auxiliary |
| Row-wise / Col-wise Search (sorted matrix) | O(1) / O(m + n) / O(m + n) | O(1) |
| Binary Search (strictly sorted flat) | O(1) / O(log(m*n)) / O(log(m*n)) | O(1) |

Java matrices are not guaranteed to be row-major contiguous blocks in memory; each row is a separate object, though row-by-row iteration still benefits cache locality.

## 3. When to Use
- Grid-based games, board simulations (e.g., Chess, Sudoku, Tic-tac-toe), or image representations.
- Dynamic programming state tables with two parameters (e.g., `dp[i][j]`).
- Implicit graph problems where cells are vertices and 4-way or 8-way adjacent cells are edges.
- Topographical problems (islands, shortest path in a maze, flood fill).

## 4. When NOT to Use
- Sparse data where most coordinates are zero (use coordinate compression or `Map<Point, Value>`).
- Dynamic row or column resizing on both axes frequently (use nested ArrayLists or specialized sparse matrices).
- 1D linear data that does not possess coordinate semantics.

## 5. Why It Works
Coordinate navigation uses directional offset vectors `DIRS = {{-1,0}, {1,0}, {0,-1}, {0,1}}` to systematically visit neighbors. Converting between 2D coordinates `(r, c)` and a 1D flat index `idx` is bijective via `idx = r * cols + c` and `r = idx / cols`, `c = idx % cols`.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Brute Force (Full Scan) | Scan all m x n cells for an element in sorted matrix | O(m * n) | O(1) |
| Optimized (Staircase Search) | Start at top-right corner; step left if target < val, down if target > val | O(m + n) | O(1) |

Staircase search eliminates an entire row or column at every step by exploiting monotonicity along both dimensions.

## 7. Data Structures Used Here
- `int[][]`: Array of array references in Java.
- `ArrayDeque<int[]>`: Used for BFS queue holding `[row, col]` coordinate pairs.

## 8. Core Template (Java)
```java
// Standard 4-directional matrix exploration template
int[][] grid = {{1, 2}, {3, 4}};
int m = grid.length, n = grid[0].length;
int[][] dirs = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};

for (int r = 0; r < m; r++) {
    for (int c = 0; c < n; c++) {
        for (int[] d : dirs) {
            int nr = r + d[0], nc = c + d[1];
            if (nr >= 0 && nr < m && nc >= 0 && nc < n) {
                // Valid neighbor grid[nr][nc]
            }
        }
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Strings](../02-strings/README.md) | [Linked List](../04-linked-list/README.md)
