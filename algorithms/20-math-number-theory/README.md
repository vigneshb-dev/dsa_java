# Math & Number Theory

> Arithmetic and number theoretic algorithms: GCD, prime sieves, modular arithmetic, and combinatorics.

## 1. Overview
Math and Number Theory algorithms solve problems involving divisibility, primes, modular congruences, and combinatorial counting. Essential foundations include the Euclidean algorithm for greatest common divisor (GCD), the Sieve of Eratosthenes for prime generation, fast modular exponentiation, and modular multiplicative inverses. These techniques handle large integer scales while preventing arithmetic overflow.

## 2. Time & Space Complexity
| Algorithm / Technique | Time Complexity | Auxiliary Space |
| :--- | :--- | :--- |
| Euclidean GCD (`gcd(a, b)`) | O(log(min(a, b))) | O(1) iterative |
| Primality Test (Trial Division) | O(sqrt(n)) | O(1) |
| Sieve of Eratosthenes | O(n log(log n)) | O(n) boolean array |
| Modular Exponentiation (`a^b % m`) | O(log b) | O(1) |
| Fermat's Little Theorem Modular Inverse | O(log m) | O(1) |

Modular arithmetic prevents integer overflow by taking intermediate results modulo `10^9 + 7` at every multiplication and addition step.

## 3. When to Use
- Computing GCD / LCM of integers.
- Prime generation or factor counting up to N.
- Large combinatorial counting (`nCr % MOD`) requiring modular multiplicative inverse.
- Fast computation of `x^y % MOD`.

## 4. When NOT to Use
- Direct numeric scale exceeds 64-bit `long` bounds without modular constraints (use `BigInteger`).
- General optimization problems that require dynamic programming or search rather than closed-form formulas.
- Floating-point approximations where exact integer arithmetic is not required.

## 5. Why It Works
The Euclidean algorithm works because `gcd(a, b) = gcd(b, a % b)`. Each reduction step at least halves the larger number, ensuring convergence in logarithmic steps. The Sieve works because any composite number `x <= n` must possess a prime factor `<= sqrt(n)`, so crossing off multiples of primes up to `sqrt(n)` eliminates all composites.

## 6. Brute Force vs Optimized
| Approach | Idea | Time | Space |
| :--- | :--- | :--- | :--- |
| Trial Division Primes up to N | Test each number from 2 to N for divisors | O(n * sqrt(n)) | O(1) |
| Sieve of Eratosthenes | Mark composite multiples iteratively | O(n log(log n)) | O(n) |

The sieve trades a boolean array of size N to avoid individual square-root trial divisions for each number.

## 7. Data Structures Used Here
- `boolean[] isPrime`: Sieve marker array.
- `long`: Used for intermediate multiplication results to prevent 32-bit signed integer overflow.

## 8. Core Template (Java)
```java
// 1. Greatest Common Divisor (Euclidean algorithm)
int gcd(int a, int b) {
    while (b != 0) {
        int temp = b;
        b = a % b;
        a = temp;
    }
    return a;
}

// 2. Fast Modular Exponentiation (base^exp % mod)
long modPow(long base, long exp, long mod) {
    long res = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) res = (res * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return res;
}
```

## 9. Problems in This Folder
| Done | File | Difficulty | Notes |
| :---: | :--- | :--- | :--- |
| [ ] | `P0000_ExampleProblem.java` | Easy | Example template placeholder |

## 10. Navigation
[Main README](../../README.md) | [Progress](../../PROGRESS.md) | [Bit Manipulation](../19-bit-manipulation/README.md) | [Matrix Algorithms](../21-matrix-algorithms/README.md)
