# Bit Manipulation
> Low-level algebraic bitwise transformations operating directly on binary numeral representations.

## 1. Overview
Bit Manipulation operates directly on the binary representations of integers using hardware-native bitwise operations (`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`). It provides constant-time $O(1)$ solutions for subset generation, parity checking, arithmetic simulation, and discovering unique elements via algebraic cancellation.

## 2. Input / Output
- Input: One or more integers (e.g. `nums = [4, 1, 2, 1, 2]`).
- Output: An integer or boolean result (e.g. single unique element `4`).

## 3. Constraints
- Operates on 32-bit (`int`) or 64-bit (`long`) integers.
- All bitwise operations execute in a single processor instruction cycle ($O(1)$ time).

## 4. Brute-Force Approach
- Idea: Convert integers to binary string representations (`Integer.toBinaryString(n)`) and parse characters sequentially, or use a `HashSet` to count duplicate occurrences.
- Pseudocode: `Set<Integer> set = new HashSet<>(); ...`
- Time: $O(n)$ time with hash allocations or $O(32)$ string parsing; Space: $O(n)$ or $O(32)$.

## 5. Optimal Approach
- Idea: Leverage core algebraic bit identities:
  - $x \oplus x = 0$ and $x \oplus 0 = x$ (XOR cancellation).
  - $n \ \& \ (n - 1)$ strips the lowest set bit (Brian Kernighan's Algorithm).
  - $n \ \& \ (-n)$ isolates the least significant bit (LSB).
```java
// Reusable Bit Manipulation Utility Template
public class BitManipulationTemplate {
    // 1. Single Number: finds unique element among duplicate pairs via XOR
    public static int singleNumber(int[] nums) {
        int xor = 0;
        for (int num : nums) xor ^= num;
        return xor;
    }

    // 2. Hamming Weight: counts set bits via Brian Kernighan's Algorithm
    public static int countSetBits(int n) {
        int count = 0;
        while (n != 0) {
            n &= (n - 1); // Clears the lowest set bit in O(1)
            count++;
        }
        return count;
    }

    // 3. Power of Two Check
    public static boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }

    // 4. Bitmask Subset Iteration: iterates all submasks of a mask
    public static void iterateSubmasks(int mask) {
        for (int sub = mask; sub > 0; sub = (sub - 1) & mask) {
            // Process submask
        }
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes string allocations, object boxing, hash table collisions, and loop iterations over unset bits.
- **Cases skipped:** Brian Kernighan's algorithm skips all 0-bits entirely, executing only as many loop iterations as there are 1-bits.
- **Shortcuts / tricks used:** XOR self-inverse property cancels duplicate pairs in a single accumulator variable.
- **Time saved:** $O(n)$ hash lookups $\to O(n)$ primitive XOR; $O(32)$ bits scan $\to O(k)$ where $k$ is number of set bits.
- **Space effect:** $O(n) \to O(1)$ space; completely eliminates auxiliary hash tables.
- **Trade-off:** Limited to word-size constraints (32 or 64 bits).

## 6. Core Idea
Integers are 32-slot boolean arrays executed directly on ALU registers. Bitwise logic allows parallel evaluation of 32 truth values in a single clock cycle.

## 7. Pattern
- Pattern: Bit Masking / XOR Cancellation / Bitwise Arithmetic.
- Signals: "Single number appearing once while others appear twice", "count number of 1 bits", "power of two", "subsets generation", "reverse bits", "bitwise AND of numbers range".

## 8. Data Structure Used
- Primitive scalar types: `int` (32 bits) and `long` (64 bits). Zero heap allocations.

## 9. Invariant
In XOR accumulation, `xor` stores the bitwise sum modulo 2 of all elements processed so far.

## 10. Dry Run
Counting set bits of $n = 12$ (`1100` in binary):
| Iteration | `n` (binary) | `n - 1` (binary) | `n & (n - 1)` (binary) | `count` |
|---|---|---|---|---|
| Init | 12 (`1100`) | - | - | 0 |
| 1 | 12 (`1100`) | 11 (`1011`) | 8 (`1000`) | 1 |
| 2 | 8 (`1000`) | 7 (`0111`) | 0 (`0000`) | 2 |

Terminates in 2 steps for 2 set bits!

## 11. Edge Cases
- Negative numbers in right shifts: signed right shift `>>` preserves sign bit (sign extension); unsigned right shift `>>>` fills with zeroes.
- Overflow when shifting by $\ge 32$: in Java, `1 << 32 == 1 << 0 == 1` because shift operand is masked with `0x1F` (use `1L << 32` for `long`).
- Integer `Integer.MIN_VALUE` ($-2^{31}$): `-n` equals itself; `isPowerOfTwo` handles via `n > 0`.

## 12. Correctness
Proved by boolean algebra: $n - 1$ flips the lowest set bit of $n$ and all trailing zeroes to ones. Therefore, bitwise AND $n \ \& \ (n - 1)$ leaves all higher bits unchanged and zeros out the lowest set bit.

## 13. Time Complexity
- Bitwise operators: $O(1)$ single instruction cycle.
- Brian Kernighan's bit count: $O(k)$ where $k \le 32$ is the number of set bits.
- Array XOR pass: $O(n)$ time.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ strictly constant memory.

## 15. Can It Be Optimized?
Hardware-intrinsic instruction `Integer.bitCount(n)` maps directly to the CPU `POPCNT` instruction, counting set bits in a single machine cycle.

## 16. When Should I Use This Algorithm?
- Finding unique elements where duplicates appear an even number of times.
- Checking powers of two, four, or eight.
- Counting set bits (Hamming Weight) or Hamming Distance between numbers.
- Generating all $2^n$ subsets without recursion.
- Space-efficient visited tracking for small states ($N \le 64$).

## 17. When Should I NOT Use It?
- State spaces exceeding 64 items (use `java.util.BitSet` or boolean arrays).
- Non-integer continuous data (floats, doubles).
- Complex relationships that cannot be modeled as boolean membership.

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic bit manipulation problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [18 - String Algorithms](../18-string-algorithms/README.md) | [20 - Math & Number Theory](../20-math-number-theory/README.md)
