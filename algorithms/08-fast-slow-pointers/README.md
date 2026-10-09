# Fast & Slow Pointers
> Floyd's Cycle-Finding Algorithm (Tortoise and Hare) utilizing dual pointer velocity differentials.

## 1. Overview
The Fast and Slow Pointers pattern (also known as Floyd's Tortoise and Hare algorithm) advances two pointers through a sequence or state graph at different speeds—typically one stepping by 1 and the other by 2. It detects cycles, discovers cycle entry points, and identifies sequence midpoints in $O(n)$ time without modifying the sequence or allocating extra memory.

## 2. Input / Output
- Input: Head node of a linked list or an implicit functional graph array (e.g. `head -> 1 -> 2 -> 3 -> 4 -> 2...`).
- Output: Boolean cycle flag, middle node, or cycle entrance node (e.g. `true` or Node `2`).

## 3. Constraints
- Scales to $n \le 10^7$ nodes/steps with $O(1)$ auxiliary memory.
- Pointer transitions must be deterministic (functional mapping $x \mapsto f(x)$).

## 4. Brute-Force Approach
- Idea: Store every visited node address in a `HashSet`. If a node is already present in the set, a cycle exists.
- Pseudocode:
  ```java
  Set<ListNode> visited = new HashSet<>();
  while (curr != null) {
      if (!visited.add(curr)) return true;
      curr = curr.next;
  }
  return false;
  ```
- Time: $O(n)$; Space: $O(n)$ auxiliary hash set memory.

## 5. Optimal Approach
- Idea: Advance `slow` by 1 node and `fast` by 2 nodes per tick. If a cycle exists, `fast` will inevitably lap and collide with `slow` inside the loop.
```java
// Reusable Floyd's Cycle Detection Skeleton
public class FastSlowPointersTemplate {
    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
    }

    public static boolean hasCycle(ListNode head) {
        if (head == null || head.next == null) return false;

        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;         // 1 step
            fast = fast.next.next;    // 2 steps

            if (slow == fast) {
                return true; // Collision confirms cycle existence
            }
        }
        return false; // Fast reached terminal null (linear path)
    }

    // Finds the exact entry node of the cycle
    public static ListNode detectCycleEntry(ListNode head) {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
            if (slow == fast) {
                // Phase 2: Reset one pointer to head; advance both by 1
                ListNode ptr1 = head;
                ListNode ptr2 = slow;
                while (ptr1 != ptr2) {
                    ptr1 = ptr1.next;
                    ptr2 = ptr2.next;
                }
                return ptr1; // Meeting point is cycle entrance
            }
        }
        return null;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes hash table insertions, rehashing, and object reference hash computations.
- **Cases skipped:** Does not need to remember past states; relative distance between pointers shrinks by 1 in every iteration within the loop.
- **Shortcuts / tricks used:** Pointer velocity difference ($2 - 1 = 1$) guarantees that inside a cycle of length $C$, the distance between fast and slow decreases by 1 each step, ensuring collision in $< C$ steps.
- **Time saved:** Retains linear $O(n)$ time while dramatically lowering constant factor overhead.
- **Space effect:** $O(n) \to O(1)$ space; completely eliminates the auxiliary hash set.
- **Trade-off:** Fast pointer performs extra node hops, but overall operations remain $O(n)$.

## 6. Core Idea
Inside a circular track, a runner moving twice as fast as another will catch and lap the slower runner within one full cycle lap. Setting both runners at the start guarantees an eventual collision if and only if a closed loop exists.

## 7. Pattern
- Pattern: Fast & Slow Pointers / Floyd's Cycle Detection.
- Signals: "Linked list cycle", "find middle of linked list", "happy number", "find duplicate number in array without modifying array", "palindrome linked list".

## 8. Data Structure Used
- Two node reference pointers (`slow`, `fast`). Zero heap allocations ($O(1)$ space).

## 9. Invariant
Inside a cycle of length $C$, the modular distance `(fast - slow) % C` decreases by exactly 1 on every iteration until reaching 0 (collision).

## 10. Dry Run
List `1 -> 2 -> 3 -> 4 -> 2` (cycle length 3, tail connects to 2):
| Iteration | `slow` (node) | `fast` (node) | Distance in Cycle | Match? |
|---|---|---|---|---|
| Init | 1 | 1 | - | - |
| 1 | 2 | 3 | 1 | No |
| 2 | 3 | 2 | 2 | No |
| 3 | 4 | 4 | 0 | **Collision!** Cycle detected |

Phase 2 Entry Search: `ptr1` at 1, `ptr2` at 4. Step 1: `ptr1` reaches 2, `ptr2` reaches 2 $\implies$ entry at Node 2.

## 11. Edge Cases
- `head == null` or single node with `head.next == null`: loop guard terminates cleanly.
- Even vs odd length lists when finding midpoints: check whether `fast.next == null` (odd length) or `fast == null` (even length).
- Cycles of length 1 (self-loop): `fast` and `slow` collide on first step.

## 12. Correctness
Let distance from head to cycle entrance be $L$, and cycle length be $C$. When slow enters cycle, fast is at distance $k = L \pmod C$. Fast catches slow in $C - k$ steps. Total steps $\le L + C$. In Phase 2, advancing `ptr1` from head and `ptr2` from meeting point by 1 step causes them to meet exactly at cycle entrance after $L$ steps.

## 13. Time Complexity
- Best: $O(1)$ (no cycle with single node).
- Average / Worst: $O(n)$ steps (at most $2n$ total pointer dereferences).

## 14. Space Complexity
- Auxiliary Space: $O(1)$ strictly constant memory.

## 15. Can It Be Optimized?
Brent's Cycle-Finding Algorithm uses powers-of-two teleportation and finds cycles slightly faster in fewer pointer steps while maintaining $O(1)$ space.

## 16. When Should I Use This Algorithm?
- Detecting cycles in linked lists.
- Finding the entry point of a loop in a linked list.
- Finding the middle node of a linked list in a single pass.
- Finding the duplicate number in an array of $n + 1$ integers in range $[1, n]$ (treat array as linked list $i \mapsto nums[i]$).
- Determining if a number is a "Happy Number".

## 17. When Should I NOT Use It?
- General directed graphs with arbitrary multiple branching out-degrees (use Kahn's topological sort or 3-color DFS).
- Tree structures (trees are acyclic by definition).
- When list can be modified and memory is unconstrained (marking node values can also detect cycles).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic fast and slow pointers placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [07 - Two Pointers](../07-two-pointers/README.md) | [09 - Sliding Window](../09-sliding-window/README.md)
