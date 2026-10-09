# Matrix Algorithms
> In-place geometric transformations, boundary sweeps, and coordinate traversals over 2D arrays.

## 1. Overview
Matrix Algorithms process two-dimensional grids to execute in-place geometric operations, rotations, boundary sweeps, and state markings. Key patterns include 90-degree in-place rotation (Transpose + Reverse), spiral boundary traversals, diagonal sweeps, and in-place row/column marking (Set Matrix Zeroes).

## 2. Input / Output
- Input: An $M \times N$ matrix (e.g. `matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]`).
- Output: In-place transformed matrix or flat sequence list (e.g. clockwise rotated `[[7, 4, 1], [8, 5, 2], [9, 6, 3]]`).

## 3. Constraints
- Dimensions $M, N \le 1000 \implies M \times N \le 10^6$ total cells.
- In-place transformations require strictly $O(1)$ auxiliary memory.

## 4. Brute-Force Approach
- Idea: Allocate a secondary auxiliary matrix of size $M \times N$, compute mapped target coordinates, copy values, and reassign back.
- Pseudocode: `int[][] copy = new int[n][n]; for(...) copy[j][n - 1 - i] = matrix[i][j];`
- Time: $O(M \cdot N)$; Space: $O(M \cdot N)$ extra allocated memory.

## 5. Optimal Approach
- Idea: Decompose geometric rotation into two symmetric algebraic operations:
  1. Transpose the matrix across the main diagonal (`matrix[i][j] <-> matrix[j][i]`).
  2. Reverse each row horizontally using two pointers.
```java
// Reusable In-Place Matrix Transformation Skeleton (Rotate 90 Degrees Clockwise)
public class MatrixAlgorithmsTemplate {
    public static void rotate(int[][] matrix) {
        int n = matrix.length;

        // Step 1: Transpose matrix across main diagonal (i < j)
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        // Step 2: Reverse each row horizontally in-place
        for (int i = 0; i < n; i++) {
            int left = 0, right = n - 1;
            while (left < right) {
                int temp = matrix[i][left];
                matrix[i][left] = matrix[i][right];
                matrix[i][right] = temp;
                left++;
                right--;
            }
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes full-grid memory duplication and allocation latency.
- **Cases skipped:** Diagonal transposition only swaps elements where $j > i$, skipping already processed cells and diagonal invariants ($i == j$).
- **Shortcuts / tricks used:** Linear algebra identity: Clockwise Rotation $90^\circ$ = Transposition followed by Horizontal Reflection.
- **Time saved:** Retains linear time $O(M \cdot N)$ while reducing memory allocations to zero.
- **Space effect:** $O(M \cdot N) \to O(1)$ auxiliary space.
- **Trade-off:** Mutates input array in-place.

## 6. Core Idea
Complex spatial 2D rotations can be broken down into elementary symmetric reflections (transpositions and row reversals) that operate purely via pair-swaps in $O(1)$ extra space.

## 7. Pattern
- Pattern: In-Place Coordinate Reflection / Boundary Layer Shrink.
- Signals: "Rotate image in-place", "spiral matrix traversal", "set matrix zeroes without extra memory", "game of life in-place", "diagonal traverse".

## 8. Data Structure Used
- 2D primitive array `int[][]` modified in-place.
- Boundary pointers (`top`, `bottom`, `left`, `right`) for spiral traversal.

## 9. Invariant
In Transpose, every element above the diagonal `(i < j)` is swapped with its reflection `(j, i)` exactly once. In Spiral traversal, boundaries strictly contract after each side is consumed.

## 10. Dry Run
Rotating $2 \times 2$ matrix `[[1, 2], [3, 4]]`:
| Operation | Target Cells | Action | Result Matrix |
|---|---|---|---|---|
| Transpose | `(0, 1)` and `(1, 0)` | Swap 2 and 3 | `[[1, 3], [2, 4]]` |
| Reverse Row 0 | `matrix[0]` | Swap 1 and 3 | `[[3, 1], [2, 4]]` |
| Reverse Row 1 | `matrix[1]` | Swap 2 and 4 | `[[3, 1], [4, 2]]` |

Result: 90 degrees clockwise rotation achieved in-place!

## 11. Edge Cases
- Non-square matrix ($M \ne N$): in-place rotation is not geometrically possible in the same buffer (requires allocating new dimensions $N \times M$).
- Single cell matrix ($1 \times 1$): no swaps performed.
- Spiral matrix with odd row/column counts: boundary condition `top <= bottom && left <= right` prevents duplicate processing of center elements.

## 12. Correctness
By linear algebra coordinate mapping:
A $90^\circ$ clockwise rotation maps coordinate $(i, j) \mapsto (j, n - 1 - i)$.
Transposition maps $(i, j) \mapsto (j, i)$.
Horizontal reversal maps $(j, i) \mapsto (j, n - 1 - i)$.
The composition of these two transformations matches the target rotation identically.

## 13. Time Complexity
- Best / Average / Worst: $O(M \cdot N)$ linear in total cell count. Every cell is visited at most twice.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ strictly in-place.

## 15. Can It Be Optimized?
Time complexity $O(M \cdot N)$ and space $O(1)$ are mathematically optimal since every cell must be moved and no extra memory is consumed.

## 16. When Should I Use This Algorithm?
- Rotating images or game boards by 90, 180, or 270 degrees in-place.
- Spiral order scanning or matrix construction.
- Setting rows and columns to zero using first row/col as state flags (Set Matrix Zeroes in $O(1)$ space).
- Cellular automata state transitions (Conway's Game of Life in-place state bit encoding).
- Zig-zag and diagonal matrix scans.

## 17. When Should I NOT Use It?
- Graph pathfinding across grids (use BFS/DFS or 2D DP).
- Sparse matrices where most cells are zeroes (use Compressed Sparse Row format).
- Massive datasets distributed across disks where non-contiguous column access causes cache thrashing.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic matrix algorithm problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [20 - Math & Number Theory](../20-math-number-theory/README.md) | End
