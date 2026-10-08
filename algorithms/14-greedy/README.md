# Greedy

> Makes locally optimal choices at each stage with the goal of reaching a global optimum.

## 1. Overview
A Greedy algorithm constructs a solution step-by-step by making the locally optimal choice at each decision point without backtracking. Greedy algorithms are correct only when the problem exhibits the greedy-choice property (local choices lead to a global optimum) and optimal substructure. When applicable, greedy strategies are significantly faster and simpler than full Dynamic Programming or backtracking.

## 2. Time & Space Complexity
| Problem / Variant | Greedy Rule | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- | :--- |
| Activity Selection | Earliest finish time first | O(n log n) sort | O(1) |
| Jump Game | Track furthest reachable index | O(n) | O(1) |
| Gas Station | Reset starting candidate on negative balance | O(n) | O(1) |
| Huffman Coding | Merge lowest frequency nodes | O(n log n) | O(n) |
| Fractional Knapsack | Highest value-to-weight ratio first | O(n log n) | O(1) |

Most greedy algorithms require sorting input upfront (O(n log n)) or maintain running extremes using a PriorityQueue.

## 3. When to Use
- Problem asks for minimum/maximum optimization and local choices never need reconsideration.
- Interval scheduling where earliest completion frees resources earliest.
- Coin change with canonical denominations (e.g., standard currency systems).
- Minimum spanning tree construction (Kruskal's or Prim's algorithm).

## 4. When NOT to Use
- Making an optimal choice now restricts superior options in later stages (0/1 Knapsack, Coin Change with arbitrary denominations).
- Problem lacks mathematical proof of the greedy-choice property (use Dynamic Programming instead).
- Finding all possible solutions rather than an optimal extremum.

## 5. Why It Works
Greedy correctness is proven using an 'exchange argument': assuming an optimal solution exists that differs from the greedy choice, exchanging the first differing choice with the greedy choice results in a solution at least as good. By induction, the sequence of greedy choices matches an optimal solution.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Dynamic Programming / Recursion | Explore both taking and leaving each option | O(2^n) or O(n^2) | O(n) |
| Greedy Approach | Always select the locally best option and advance | O(n) or O(n log n) | O(1) |

Greedy discards all alternative subproblems irrevocably, replacing exponential search trees with a single linear path.

## 7. Data Structures Used Here
- `PriorityQueue`: Dynamically retrieves highest/lowest candidate.
- `Arrays.sort()`: Chronological or priority pre-sorting.

## 8. Core Template (Java)
```java
// Jump Game (Tracking furthest reachable boundary)
boolean canJump(int[] nums) {
    int maxReach = 0;
    for (int i = 0; i < nums.length; i++) {
        if (i > maxReach) return false; // unreachable cell
        maxReach = Math.max(maxReach, i + nums[i]);
        if (maxReach >= nums.length - 1) return true;
    }
    return true;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Cyclic Sort](../13-cyclic-sort/README.md) | [Dynamic Programming](../15-dynamic-programming/README.md)
