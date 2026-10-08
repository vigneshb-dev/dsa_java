# Matrix Algorithms

> Specialized transformations and traversals on 2D grids: spiral traversal, in-place rotation, and flood fill.

## 1. Overview
Matrix Algorithms focus on multi-dimensional geometric manipulations and traversal sequences over 2D grids. Classic challenges include rotating a square matrix 90 degrees in-place, spiral order traversal using moving boundary barriers, and image flood fill. These algorithms emphasize index arithmetic, boundary condition handling, and in-place swapping without allocating duplicate grids.

## 2. Time & Space Complexity
| Operation / Algorithm | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| Rotate Matrix 90 deg In-Place | O(n^2) | O(1) |
| Spiral Order Traversal | O(m * n) | O(1) auxiliary (O(m*n) output) |
| Set Matrix Zeroes In-Place | O(m * n) | O(1) |
| Transpose Matrix | O(n^2) | O(1) |
| Flood Fill / Connected Cells | O(m * n) | O(m * n) call stack |

In-place 90-degree clockwise rotation is achieved by transposing the matrix along its main diagonal followed by reversing each row.

## 3. When to Use
- Spiral, zigzag, or diagonal ordered traversals of 2D grids.
- In-place image or grid rotations and reflections under strict O(1) space constraints.
- Matrix zeroing where marker rows/columns must be stored within the grid's first row and column.
- Boundary-based flood fills and island shape analysis.

## 4. When NOT to Use
- Problems requiring shortest path distances on weighted grids (use Dijkstra or BFS).
- General graph problems where vertices have dynamic adjacency rather than geometric 4-way neighbors.
- Sparse matrices where allocating an m x n grid wastes memory.

## 5. Why It Works
Rotating a matrix 90 degrees clockwise maps coordinate `(r, c)` to `(c, n - 1 - r)`. Decomposing this affine transformation into two simple steps: 1) Transpose: swap `(r, c)` with `(c, r)`, 2) Reverse row: swap `(r, c)` with `(r, n - 1 - c)`, executes the exact rotation in-place without auxiliary memory.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Copy to New Matrix | Allocate fresh `res[n][n]` and assign `res[c][n-1-r] = matrix[r][c]` | O(n^2) | O(n^2) |
| In-Place Transpose + Reverse | Transpose across diagonal, then reverse each row horizontally | O(n^2) | O(1) |

Transposing and reversing in-place uses coordinate mapping to eliminate the O(n^2) auxiliary matrix allocation.

## 7. Data Structures Used Here
- `int[][]`: Direct 2D array representation in Java.

## 8. Core Template (Java)
```java
// In-place 90-degree clockwise matrix rotation
void rotate(int[][] matrix) {
    int n = matrix.length;
    // 1. Transpose matrix (swap matrix[i][j] with matrix[j][i])
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[j][i];
            matrix[j][i] = temp;
        }
    }
    // 2. Reverse each row
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n / 2; j++) {
            int temp = matrix[i][j];
            matrix[i][j] = matrix[i][n - 1 - j];
            matrix[i][n - 1 - j] = temp;
        }
    }
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Math & Number Theory](../20-math-number-theory/README.md) | End
