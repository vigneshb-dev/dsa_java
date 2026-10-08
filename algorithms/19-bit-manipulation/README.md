# Bit Manipulation

> Low-level bitwise operations (`&`, `|`, `^`, `~`, `<<`, `>>`, `>>>`) for compact state and arithmetic tricks.

## 1. Overview
Bit Manipulation operates directly on binary representations of integers using hardware bitwise instructions. Bitwise primitives operate in a single CPU clock cycle, delivering high execution speed and O(1) space efficiency. Core applications include tracking boolean flags as bitmasks, finding unique numbers via XOR cancellation, and isolated bit extraction.

## 2. Time & Space Complexity
| Operation / Trick | Formula / Syntax | Time Complexity | Space Complexity |
| :--- | :--- | :--- | :--- |
| Check if k-th bit is set | `(n & (1 << k)) != 0` | O(1) | O(1) |
| Set k-th bit | `n | (1 << k)` | O(1) | O(1) |
| Clear k-th bit | `n & ~(1 << k)` | O(1) | O(1) |
| Toggle k-th bit | `n ^ (1 << k)` | O(1) | O(1) |
| Clear lowest set bit | `n & (n - 1)` | O(1) | O(1) |
| Extract lowest set bit | `n & (-n)` | O(1) | O(1) |
| Count set bits (Brian Kernighan) | Loop `n = n & (n - 1)` | O(Number of set bits) | O(1) |

Java's `Integer.bitCount(n)` is compiled to hardware POPCNT instructions running in O(1) time.

## 3. When to Use
- Tracking subsets or visited state for up to 32 (or 64 with `long`) items compactly.
- Single Number problems where XOR cancels pairs (`x ^ x = 0` and `x ^ 0 = x`).
- Determining if a number is a power of two (`n > 0 && (n & (n - 1)) == 0`).
- Fast mathematical operations (e.g., multiply/divide by 2 via `<< 1` and `>> 1`).

## 4. When NOT to Use
- Set size exceeds 64 elements (use `BitSet` or `boolean[]` instead of primitive bitmasks).
- Over-complicating readable arithmetic logic where readability is preferred.
- Working with floating-point calculations where bit representations follow IEEE 754 float specifications.

## 5. Why It Works
XOR is associative, commutative, and self-inverting (`a ^ b ^ a = b`), enabling cancellation of duplicate values without auxiliary memory. Subtracting 1 from an integer flips the lowest set bit and all trailing zeroes to ones; performing `n & (n - 1)` strips that lowest set bit cleanly.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Hash Set for Single Number | Insert elements, remove duplicates | O(n) | O(n) |
| XOR Accumulator | XOR all elements together; duplicates cancel to 0 | O(n) | O(1) |

XOR accumulation exploits binary cancellation to completely eliminate auxiliary hash set memory.

## 7. Data Structures Used Here
- `int` / `long`: 32-bit / 64-bit primitive integer holding bit flags.
- `java.util.BitSet`: Dynamically sized vector of bits for sets larger than 64.

## 8. Core Template (Java)
```java
// Useful bit manipulation routines
// 1. Single Number using XOR
int singleNumber(int[] nums) {
    int xor = 0;
    for (int num : nums) xor ^= num;
    return xor;
}

// 2. Count set bits (Brian Kernighan's Algorithm)
int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n &= (n - 1); // clears lowest set bit
        count++;
    }
    return count;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [String Algorithms](../18-string-algorithms/README.md) | [Math & Number Theory](../20-math-number-theory/README.md)
