# Prefix Sum & Difference Array

> Precomputed cumulative sums for O(1) range queries and difference markers for O(1) range updates.

## 1. Overview
Prefix Sum precomputes running cumulative totals across an array so that any subarray sum `nums[i..j]` can be evaluated in constant O(1) time via subtraction. Conversely, a Difference Array records increments between adjacent elements, allowing multiple range additions `[L, R] += val` to be executed in O(1) time by modifying endpoints only. Together, they form standard building blocks for static range queries and batch interval updates.

## 2. Time & Space Complexity
| Technique | Operation | Precomputation Time | Query / Update Time | Space |
| :--- | :--- | :--- | :--- | :--- |
| Prefix Sum | Range Sum `[i..j]` | O(n) | O(1) query | O(n) |
| 2D Prefix Sum | Submatrix Sum `[r1,c1..r2,c2]` | O(m * n) | O(1) query | O(m * n) |
| Difference Array | Range Update `[L..R] += val` | O(n) init | O(1) update, O(n) restore | O(n) |
| Prefix Sum + HashMap | Subarrays with Sum == K | O(n) single pass | O(1) lookup per element | O(n) |

A 1-indexed prefix array `prefix[i] = prefix[i-1] + nums[i-1]` prevents index-out-of-bounds checks for range queries starting at index 0.

## 3. When to Use
- Frequent range sum queries on an array that does not undergo point updates.
- Counting subarrays whose sum equals K (using `prefixSum - k` hash map lookup).
- Batch range updates where modifications are done upfront before final array inspection.
- Submatrix rectangle sum queries in static 2D grids.

## 4. When NOT to Use
- Array undergoes point updates between range queries (use Fenwick Tree or Segment Tree).
- Queries ask for range minimum or maximum instead of cumulative sum (use Sparse Table or Segment Tree).
- Range operations are multiplicative with zeroes or non-invertible operators.

## 5. Why It Works
Prefix sum works because addition is invertible: `Sum(i..j) = Prefix(j) - Prefix(i - 1)`. Difference array works because taking prefix sums of the difference array cancels intermediate terms: adding `val` at `L` and subtracting `val` at `R + 1` increases the prefix sum strictly within `[L, R]`.

## 6. Brute Force vs Optimized
| Approach | Range Query `[L..R]` | Range Update `[L..R]` | Space |
| :--- | :--- | :--- | :--- |
| Brute Force | O(n) loop from L to R | O(n) loop from L to R | O(1) |
| Prefix Sum / Diff Array | O(1) via `P[R] - P[L-1]` | O(1) via `D[L] += v, D[R+1] -= v` | O(n) |

Precomputing cumulative prefixes trades O(n) memory to reduce range queries and batch updates to O(1).

## 7. Data Structures Used Here
- `int[] prefix`: Array of size `n + 1` storing cumulative totals.
- `int[] diff`: Array of size `n + 2` storing endpoint deltas.
- `HashMap<Integer, Integer>`: Stores prefix sum occurrences for subarray sum target counting.

## 8. Core Template (Java)
```java
// 1. Prefix Sum for O(1) Range Sum Queries
int[] nums = {1, 2, 3, 4, 5};
int[] prefix = new int[nums.length + 1];
for (int i = 0; i < nums.length; i++) {
    prefix[i + 1] = prefix[i] + nums[i];
}
// Query range [l, r] (0-indexed inclusive)
int rangeSum = prefix[r + 1] - prefix[l];

// 2. Difference Array for O(1) Range Updates
int[] diff = new int[nums.length + 1];
// Add val to range [l, r]
diff[l] += val;
if (r + 1 < nums.length) diff[r + 1] -= val;
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Sliding Window](../09-sliding-window/README.md) | [Monotonic Stack & Queue](../11-monotonic-stack-queue/README.md)
