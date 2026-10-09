# Math & Number Theory
> Prime factorization, modular arithmetic, greatest common divisors, and fast binary exponentiation.

## 1. Overview
Math & Number Theory algorithms solve algebraic and discrete arithmetic problems fundamental to cryptography, combinatorics, and modular arithmetic. Foundational techniques include the Sieve of Eratosthenes ($O(N \log \log N)$ prime generation), Euclidean Algorithm ($O(\log(\min(a, b)))$ GCD), and Binary Exponentiation ($O(\log b)$ power computation).

## 2. Input / Output
- Input: Positive integers $a, b, n$ (e.g. $a = 2, b = 10, m = 10^9 + 7$).
- Output: Greatest common divisor, boolean primality, or evaluated modular power (e.g. $2^{10} = 1024$).

## 3. Constraints
- Modulo computations typically use $M = 10^9 + 7$ or $998244353$ (prime moduli).
- Fast exponentiation scales to powers up to $b = 10^{18}$ in under 60 operations.
- Sieve of Eratosthenes operates up to $N \le 10^7$ within 10MB of boolean memory.

## 4. Brute-Force Approach
- Idea: Linear loop multiplying $b$ times for power, or trial division up to $n$ for primality testing.
- Pseudocode: `long ans = 1; for (int i = 0; i < b; i++) ans = (ans * a) % m;`
- Time: $O(b)$ for power, $O(n)$ for primality; Space: $O(1)$.

## 5. Optimal Approach
- Idea: Decompose power $b$ into binary bits (halving power when even, squaring base). For GCD, apply Euclidean remainder theorem recursively.
```java
// Reusable Number Theory Utility Template
public class MathNumberTheoryTemplate {
    // 1. Fast Exponentiation: computes (base^exp) % mod in O(log exp)
    public static long power(long base, long exp, long mod) {
        long res = 1;
        base %= mod;
        while (exp > 0) {
            if ((exp & 1) == 1) res = (res * base) % mod; // Odd bit: multiply
            base = (base * base) % mod;                   // Square base
            exp >>= 1;                                    // Halve power
        }
        return res;
    }

    // 2. Euclidean GCD: computes gcd(a, b) in O(log(min(a, b)))
    public static long gcd(long a, long b) {
        return b == 0 ? a : gcd(b, a % b);
    }

    public static long lcm(long a, long b) {
        return (a / gcd(a, b)) * b;
    }

    // 3. Sieve of Eratosthenes: finds all primes up to n in O(n log log n)
    public static boolean[] sieve(int n) {
        boolean[] isPrime = new boolean[n + 1];
        java.util.Arrays.fill(isPrime, true);
        isPrime[0] = isPrime[1] = false;

        for (int p = 2; p * p <= n; p++) {
            if (isPrime[p]) {
                for (int i = p * p; i <= n; i += p) {
                    isPrime[i] = false;
                }
            }
        }
        return isPrime;
    }
}
```

### 5A. How the Optimized Approach Reduces Time and Space
- **Work eliminated:** Removes $b - \log_2 b$ multiplications in exponentiation; eliminates checking multiples of non-prime composites in the sieve.
- **Cases skipped:** Starting composite marking at $p^2$ skips multiples already eliminated by smaller prime factors.
- **Shortcuts / tricks used:** Binary halving reduces power in logarithmic steps; modulo property $(a \cdot b) \pmod m = ((a \pmod m) \cdot (b \pmod m)) \pmod m$.
- **Time saved:** Power: $O(b) \to O(\log b)$; Primality: $O(n) \to O(\sqrt{n})$ or $O(n \log \log n)$ bulk.
- **Space effect:** Allocates $O(n)$ boolean array for bulk sieve; $O(1)$ for GCD and exponentiation.
- **Trade-off:** Sieve memory bounds practical limits to $N \le 10^7$.

## 6. Core Idea
Algebraic and arithmetic structures allow exponential step reductions: binary representation decomposes powers into sums of powers of two, and Euclidean division replaces subtraction chains with modulo arithmetic.

## 7. Pattern
- Pattern: Binary Exponentiation / Prime Sieve / Euclidean Reduction.
- Signals: "Pow(x, n)", "modulo 10^9+7", "greatest common divisor", "count primes up to n", "modular multiplicative inverse (Fermat's Little Theorem)".

## 8. Data Structure Used
- Primitive scalar variables `long`.
- `boolean[] isPrime` array (or `BitSet` for memory-constrained sieving).

## 9. Invariant
In Euclidean algorithm, $\gcd(a, b) = \gcd(b, a \pmod b)$ remains invariant at each step. In fast power, $\text{res} \times \text{base}^{\text{exp}} \equiv a^b \pmod m$ holds true throughout the loop.

## 10. Dry Run
Computing $2^{10}$ via Binary Exponentiation:
| Iteration | `exp` | `exp & 1` | `res` | `base` (squared) | Next `exp` |
|---|---|---|---|---|---|
| Init | 10 (`1010`) | - | 1 | 2 | 10 |
| 1 | 10 | 0 | 1 | $2^2 = 4$ | 5 |
| 2 | 5 (`101`) | 1 | $1 \times 4 = 4$ | $4^2 = 16$ | 2 |
| 3 | 2 (`10`) | 0 | 4 | $16^2 = 256$ | 1 |
| 4 | 1 (`1`) | 1 | $4 \times 256 = 1024$ | $256^2$ | 0 |

Result: 1024 in 4 steps!

## 11. Edge Cases
- Negative exponents in `pow(x, n)`: invert base $x = 1/x$ and use $-n$ (watch for `Integer.MIN_VALUE` overflow by casting to `long`).
- Zero power $a^0 = 1$; $\gcd(a, 0) = a$.
- Multiplication overflow: multiplying two 32-bit integers modulo $M$ requires intermediate `long` casting to prevent 32-bit overflow before modulo is applied.

## 12. Correctness
By algebraic identities: $a^b = (a^2)^{b/2}$ for even $b$, and $a \cdot a^{b-1}$ for odd $b$. Euclidean division $a = qb + r$ guarantees any common divisor of $a$ and $b$ also divides $r = a - qb$.

## 13. Time Complexity
- Binary Exponentiation: $O(\log b)$.
- Euclidean GCD: $O(\log(\min(a, b)))$.
- Sieve of Eratosthenes: $O(n \log \log n)$.

## 14. Space Complexity
- Auxiliary Space: $O(1)$ for power and GCD; $O(n)$ for Sieve array.

## 15. Can It Be Optimized?
Linear Sieve (Euler's Sieve) guarantees each composite is marked exactly once by its smallest prime factor, achieving strict $O(n)$ time. Matrix Exponentiation extends binary power to linear recurrence vectors in $O(k^3 \log n)$.

## 16. When Should I Use This Algorithm?
- Computing large powers modulo $M$ (cryptography, competitive programming).
- Finding greatest common divisor and least common multiple.
- Modular Multiplicative Inverse using Fermat's Little Theorem ($a^{M-2} \equiv a^{-1} \pmod M$ for prime $M$).
- Bulk prime generation up to $10^7$ (Sieve).
- Calculating combinations $\binom{n}{k} \pmod M$ via precomputed factorials.

## 17. When Should I NOT Use It?
- Testing single large number primality where $N > 10^9$ (use Miller-Rabin probabilistic test in $O(k \log^3 N)$).
- Factoring enormous integers $N > 10^{14}$ (use Pollard's rho algorithm).
- Simple integer addition and subtraction without modulo overflow.

---

## Comparison Table
| Algorithm | Time | Space | Primary Domain | Best Use |
|---|---|---|---|---|
| Sieve of Eratosthenes | $O(n \log \log n)$ | $O(n)$ | Integers $\le 10^7$ | Precomputing primes, prime factorization queries |
| Euclidean GCD | $O(\log(\min(a, b)))$ | $O(1)$ | 64-bit integers | Simplifying fractions, coprimality, LCM |
| Fast Exponentiation | $O(\log b)$ | $O(1)$ | Powers up to $10^{18}$ | Modular powers, modular inverses, matrix powers |

---

### Algorithm: Sieve of Eratosthenes
- **Input / Output:** Integer $n$ $\to$ Boolean array of prime flags up to $n$.
- **Constraints:** $n \le 10^7$.
- **Brute Force:** Trial division for all numbers up to $n$ ($O(n \sqrt{n})$).
- **Optimal Approach:** Mark multiples of each prime starting at $p^2$ with step $p$.
- **How It Reduces Time/Space:** Skips all composite multiples; harmonic prime sum is $O(n \log \log n)$.
- **Core Idea:** Every composite has a prime factor $\le \sqrt{n}$.
- **Pattern:** Multiple elimination sieve.
- **Data Structure Used:** `boolean[] isPrime`.
- **Invariant:** Unmarked numbers up to $p$ are verified primes.
- **Dry Run:** Marks multiples of 2, 3, 5; remaining true values are primes.
- **Edge Cases:** 0 and 1 are non-prime.
- **Correctness:** By Fundamental Theorem of Arithmetic.
- **Time Complexity:** $O(n \log \log n)$.
- **Space Complexity:** $O(n)$.
- **Can It Be Optimized:** Linear Sieve achieves $O(n)$ time.
- **When to Use:** Bulk prime lookups, finding prime factors of numbers up to $10^7$.
- **When NOT to Use:** Testing single large numbers $n > 10^9$ (use Miller-Rabin).

### Algorithm: Euclidean Algorithm (GCD)
- **Input / Output:** Two integers $a, b$ $\to$ Greatest common divisor.
- **Constraints:** $a, b \le 10^{18}$.
- **Brute Force:** Scan down from $\min(a, b)$ to 1 ($O(\min(a, b))$).
- **Optimal Approach:** Recursive modulo step: `gcd(a, b) = gcd(b, a % b)`.
- **How It Reduces Time/Space:** Modulo cuts operands by at least half every two steps.
- **Core Idea:** Divisors of $a$ and $b$ are identical to divisors of $b$ and $a \pmod b$.
- **Pattern:** Modulo Euclidean reduction.
- **Data Structure Used:** Scalar variables.
- **Invariant:** Common divisor set of pair is preserved.
- **Dry Run:** $\gcd(48, 18) \to \gcd(18, 12) \to \gcd(12, 6) \to \gcd(6, 0) = 6$.
- **Edge Cases:** One argument 0; order of arguments handled automatically.
- **Correctness:** Euclidean division theorem.
- **Time Complexity:** $O(\log(\min(a, b)))$.
- **Space Complexity:** $O(1)$ iterative.
- **Can It Be Optimized:** Binary GCD (Stein's algorithm) avoids modulo using bit shifts.
- **When to Use:** Finding GCD, LCM, or reducing rational fractions.
- **When NOT to Use:** When inputs are already known powers of 2.

### Algorithm: Fast Exponentiation
- **Input / Output:** Base $a$, exponent $b$, mod $m$ $\to (a^b) \pmod m$.
- **Constraints:** $b \le 10^{18}, m \le 2 \times 10^9$.
- **Brute Force:** Multiply $b$ times ($O(b)$).
- **Optimal Approach:** Square base and halve exponent using bit shifts.
- **How It Reduces Time/Space:** Computes power in logarithmic steps.
- **Core Idea:** $a^b = a^{\sum b_i 2^i} = \prod (a^{2^i})^{b_i}$.
- **Pattern:** Binary power decomposition.
- **Data Structure Used:** Scalar `long` registers.
- **Invariant:** Accumulated product preserves remaining power equivalence.
- **Dry Run:** Halves power each step; multiplies accumulator on odd bits.
- **Edge Cases:** $b = 0 \to 1$; negative powers; 64-bit modulo multiplication overflow.
- **Correctness:** Binary numeral representation expansion.
- **Time Complexity:** $O(\log b)$.
- **Space Complexity:** $O(1)$.
- **Can It Be Optimized:** Optimal scalar exponentiation.
- **When to Use:** Modular exponentiation; Fermat's Little Theorem modular inverse.
- **When NOT to Use:** Small constant powers ($a^2, a^3$; direct multiplication is faster).

## 18. Problems in This Folder
| Done | File | Difficulty | Notes |
|:---:|---|---|---|
| - [ ] | `P0000_ExampleProblem.java` | Easy | Basic math/number theory problem placeholder |

## 19. Navigation
Links: [Main README](../../README.md) | [Progress](../../PROGRESS.md) | [19 - Bit Manipulation](../19-bit-manipulation/README.md) | [21 - Matrix Algorithms](../21-matrix-algorithms/README.md)
