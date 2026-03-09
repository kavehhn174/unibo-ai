<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

# This is one of the slides from the course Introduction to Computability and Complexity and I want you to consider all subjects that are required for a student to learn and e read for exams

Explain the exercises step by step and create shapes if required and write them all in a typed format that are easier to read for beginners and new commers

Explain all equations, definitions and everything in a simpler format

If there are any exercises or examples, you need to add 1 extra example like the original one and write the solution

Also make sure to simplify every definition and formula using examples like a walkthrough
should be simplified and explained that what it means exactly be clear and simple to understand all terms

Write the report in markdown

Here is the full beginner-friendly study guide based on your slides:[^1]

***

# 📘 Introduction to Computability and Complexity — Study Guide

> **Course:** Introduction to Computability and Complexity
> **Lecturer:** Ugo Dal Lago — University of Bologna, A.Y. 2025/2026

***

## Section 1 — A Bit of History

### What is Computation?

**Simple definition:** Computation is any step-by-step process that takes some input and produces an output.

Think of a recipe: you have ingredients (input), follow steps (process), and get a dish (output). Computation works the same way.[^1]

Computation is **not** a new idea — it's been around for thousands of years, long before computers existed.[^1]

> **Classic Example — Euclid's Algorithm (~300 BC)**
> Euclid figured out how to find the Greatest Common Divisor (GCD) of two numbers using a step-by-step method. That's computation — even 2300 years ago![^1]
>
> **Walkthrough:** Find GCD(48, 18)
> 1. 48 ÷ 18 = 2, remainder **12**
> 2. 18 ÷ 12 = 1, remainder **6**
> 3. 12 ÷ 6 = 2, remainder **0** → STOP
> 4. GCD = **6** ✅

Before the 20th century, computation was treated more like an art than a science — people had many different, often contradictory definitions. The **Modern Theory of Computation (ToC)** changed this in the first half of the 20th century by giving a precise, scientific definition of what it means to "compute" something.[^1]

***

### Two Big Fields: Computability vs. Complexity

| Field | Main Question | Key Term |
| :-- | :-- | :-- |
| **Computability Theory** | *Can* this task be done at all? | Computable / Uncomputable |
| **Complexity Theory** | *How efficiently* can it be done? | Time / Space complexity |

**In simple terms:**

- Computability asks: *"Is it possible?"*
- Complexity asks: *"Is it practical?"*[^1]

This course focuses mainly on **computational complexity theory**.[^1]

***

### Impact of Theory of Computation

ToC has influenced two major areas since the 1940s:[^1]

1. **Other Sciences** — genomics, physics, biology use ideas from computation theory.
2. **ICT (Information and Communication Technology)** — concepts like *universality* directly shaped how computers were designed from the very beginning.

***

## Section 2 — Modelling Computation

### The Standard Paradigm

```
[ Computational Task ] <--- solved by --- [ Computational Process ]
```

- A **Task** is *what* you want to accomplish (e.g., multiply two numbers).
- A **Process** is *how* you do it (the algorithm you choose).[^1]

> **Key Insight:** Multiple processes can solve the same task. Some are fast, some are extremely slow — and that difference matters enormously.[^1]

***

### 🔢 Example 1: The Multiplication Problem

**Task:** Multiply two numbers `a` and `b`, both having `n` digits.

**Two different processes solve this:**

#### Process A — RepeatedAddition

> a × b = a + a + a + ... + a (b times)

- Adding two `n`-digit numbers takes roughly `n` steps.
- Doing this `b` times = **b × n steps total**.
- **Problem:** `b` can be as large as $10^n - 1$, making this astronomically slow.[^1]

> When n = 100: this would take more than $10^{80}$ **years**. The universe is only ~13.8 billion years old!

#### Process B — GridMethod (School Multiplication)

> The long multiplication you learned in school.

- Takes roughly **n × n = n²** steps.[^1]
- When n = 100: takes **10,000 steps** → at 1 step/millisecond = **1 second** ✅


#### Comparison Table

| Process | Steps | n=100 Time |
| :-- | :-- | :-- |
| RepeatedAddition | $b \cdot n \approx 10^n \cdot n$ | > $10^{80}$ years |
| GridMethod | $n^2$ | ~1 second |

**Conclusion:** GridMethod is so much faster that it proves multiplication belongs to the class **P** (problems solvable in practical/polynomial time).[^1]

> 🆕 **Extra Example (same idea):**
> **Task:** Add the numbers 1 through `n`.
>
> - **Process A (Loop):** Add 1+2+3+...+n one at a time → **n steps**
> - **Process B (Formula):** Use $\frac{n(n+1)}{2}$ → **1 step**
>
> For n = 1,000,000: Process A takes a million operations; Process B takes one. Same answer, totally different efficiency!

***

### 👰 Example 2: The Wedding Problem

**Task:** You have a guest list `L`. Some pairs of guests are *incompatible* (they hate each other). Find the **largest group** of guests where nobody hates anyone else.[^1]

**Naive Process — Try Every Subset:**

- Look at every possible group of guests, check for conflicts, track the biggest compatible group.
- If there are `n` guests, the number of possible subsets is **2ⁿ**.[^1]

```
n = 10  → 1,024 subsets    (fine)
n = 30  → 1,073,741,824    (slow)
n = 100 → more atoms than in the observable universe (impossible)
```

- This problem is in the class **NP**.[^1]
- The big open question: can we do *significantly better* than checking all subsets? Solving this = solving the famous **P vs. NP problem**.[^1]

> 🆕 **Extra Example (same idea):**
> **Task:** From a menu of 20 dishes, find the largest group of dishes that don't share any ingredient (all-compatible plate).
>
> Naive approach: try all $2^{20}$ = 1,048,576 subsets. Doable for a computer, but with 100 dishes this becomes impossible — same structure as the Wedding Problem.

***

### What is an Algorithm (a "Process")?

An **algorithm** must satisfy three rules:[^1]

1. ✅ **Finite description** — it must be written down in a finite number of steps (can't be infinitely long)
2. ✅ **Elementary steps** — each step must be simple and basic
3. ✅ **Deterministic** — at each step, there is only one clear "next step" (no ambiguity)

Any Python (or Java, C++) program is an algorithm at a high level. In this course, you'll use a more formal, low-level model.[^1]

***

### Why Can't We Just Try All Algorithms?

To prove a task has NO efficient algorithm, you'd need to check every possible process — but there are infinitely many. That's why proving tasks are *hard* is extremely difficult. **Complexity theory's whole goal is to find mathematical tools to do this**.[^1]

Rather than proving individual algorithms don't work, complexity theory **compares tasks to each other** — showing "if task A is solvable efficiently, then task B is too".[^1]

***

### Open Research Questions in Complexity Theory

- ❓ Is **P = NP**? (Can all easily checkable problems be easily solved?)
- ❓ Does **randomness** speed up computation?
- ❓ Can we still solve tasks if the algorithm is allowed to sometimes **make mistakes**?
- ❓ Can **quantum computers** solve problems much faster?[^1]

***

## Section 3 — Mathematical Preliminaries

### Sets and Numbers

| Symbol | Meaning | Example |
| :-- | :-- | :-- |
| `\|X\|` | Cardinality (size) of set X | `\|{a, b, c}\|` = 3 |
| **N** | Natural numbers | 0, 1, 2, 3, ... |
| **Z** | Integers | ..., -2, -1, 0, 1, 2, ... |
| `⌈x⌉` | Ceiling: smallest integer ≥ x | `⌈3.2⌉` = 4 |
| `[n]` | The set {1, 2, ..., n} | `[^4]` = {1, 2, 3, 4} |
| `log x` | In this course = **log base 2** | `log 8` = 3 |

> **"Holds for sufficiently large n"** means: there is some cutoff point N where a property becomes true for **all** n bigger than N. It's like saying "eventually, this rule always applies."[^1]

***

### Strings

A **string** is just a sequence of characters from some alphabet.[^1]

> 🔤 Think of it like a word, but the alphabet can be just `{0, 1}` (binary).


| Notation | Meaning | Example |
| :-- | :-- | :-- |
| `S` | Alphabet (set of symbols) | `{0, 1}` |
| `Sⁿ` | All strings of length exactly n | `{0,1}²` = {00, 01, 10, 11} |
| `S*` | ALL possible strings (any length) | includes ε, 0, 1, 00, 01, ... |
| `ε` | The empty string (length zero) | like an empty word |
| `\|x\|` | Length of string x | `\|000101\|` = 6 |
| `xy` | Concatenation of x and y | `01` · `10` = `0110` |
| `xᵏ` | String x repeated k times | `01³` = `010101` |

> **Example from slides:**[^1]
> - `{0,1}²` = `{00, 01, 10, 11}` — all 2-bit binary strings
> - `|000101|` = **6** — the string has six characters

> 🆕 **Extra Example:**
> Alphabet S = `{a, b}`. What is S³ (all strings of length 3)?
>
> ```> S³ = {aaa, aab, aba, abb, baa, bab, bba, bbb} >```
> That's 2³ = **8 strings** total.

***

### Functions

A **function** f: X → Y assigns to every element in X **exactly one** element in Y.[^1]

**Step-by-step breakdown:**

1. **Cartesian Product** X × Y = all pairs (x, y) where x ∈ X and y ∈ Y

> Example: X = {1, 2}, Y = {a, b}
> X × Y = {(1,a), (1,b), (2,a), (2,b)}
2. **Relation** = any subset of X × Y

> Example: {(1,a), (2,b)} is a relation
3. **Function** = a relation where every x has **exactly one** matching y[^1]

> {(1,a), (2,b)} ✅ — each input has one output
> {(1,a), (1,b), (2,a)} ❌ — input 1 maps to two outputs!

***

### Tasks as Functions

In complexity theory, every computational task is modeled as a function from binary strings to binary strings:[^1]

$$
f : \{0,1\}^* \to \{0,1\}^*
$$

**Why binary strings?** Because any data — numbers, text, graphs, images — can be **encoded** as a sequence of 0s and 1s.[^1]

***

### Encoding Objects as Strings

| Object Type | Encoding Method | Example |
| :-- | :-- | :-- |
| Alphabet `{a,b,c}` | a=00, b=01, c=10 | `abbc` → `00010110` |
| Natural numbers | Binary representation | `12` → `1100` |
| Pairs (x, y) | Use separator: `x#y` | `(01, 10)` → `01#10` |

[^1]

> 🆕 **Extra Example:**
> Encode the word `cab` using {a=00, b=01, c=10}:
> - c → 10, a → 00, b → 01
> - Result: **`100001`** ✅

***

### Languages and Decision Problems

A **language** L is just a set of strings — a subset of `{0,1}*`.[^1]

A **decision problem** asks: *"Is this string x in language L? Yes or No?"*

This maps to a **boolean function** (outputs only 0 or 1):

$$
f(x) = \begin{cases} 1 & \text{if } x \in L \\ 0 & \text{if } x \notin L \end{cases}
$$

> **Example:** Let L = all binary strings that represent **even numbers**.
> - Is `1010` (= 10) in L? → **Yes**, f("1010") = 1
> - Is `1011` (= 11) in L? → **No**, f("1011") = 0

> 🆕 **Extra Example:**
> Let L = all binary strings that start with `1`.
> - `101` → in L → f = **1**
> - `010` → not in L → f = **0**
> - `1` → in L → f = **1**

***

### Asymptotic Notation (Big-O, Omega, Theta)

These notations describe **how fast a function grows** as the input size `n` gets large. They are the language of algorithm efficiency.[^1]

***

#### 🔵 Big-O Notation — "At most this fast"

> **Definition:** f(n) is O(g(n)) if there exists a constant c > 0 such that:

> $$
f(n) \leq c \cdot g(n) \quad \text{for all sufficiently large } n
$$

**Think of it as:** an **upper bound** — f grows *no faster* than g.

> **Example from slides:** $f(n) = 3n^2 + 4n$[^1]
> - Is O(n²)? YES — $3n^2 + 4n \leq 7n^2$ for large n ✅
> - Is O(n³)? YES — n² grows slower than n³ ✅
> - Is O(2ⁿ)? YES — exponential grows even faster ✅
> - Is O(n)? **NO** — n² grows faster than n ❌

> 🆕 **Extra Example:** $f(n) = 5n + 100$
> - O(n)? **YES** — $5n + 100 \leq 105n$ for n ≥ 1 ✅
> - O(n²)? YES (n is slower than n²) ✅
> - O(log n)? **NO** — n grows faster than log n ❌

***

#### 🔴 Big-Omega (Ω) Notation — "At least this fast"

> **Definition:** f(n) is Ω(g(n)) if there exists a constant c > 0 such that:

> $$
f(n) \geq c \cdot g(n) \quad \text{for all sufficiently large } n
$$

**Think of it as:** a **lower bound** — f grows *at least as fast* as g.

> **Example from slides:** $f(n) = 3n^2 + 4n$[^1]
> - Ω(n²)? YES ✅
> - Ω(n)? YES (n² ≥ n for large n) ✅
> - Ω(n³)? **NO** — n² grows slower than n³ ❌

> 🆕 **Extra Example:** $f(n) = n^3 + 2$
> - Ω(n³)? **YES** ✅
> - Ω(n²)? YES ✅
> - Ω(2ⁿ)? **NO** — $n^3$ grows much slower than $2^n$ ❌

***

#### 🟢 Big-Theta (Θ) Notation — "Exactly this fast"

> **Definition:** f(n) is Θ(g(n)) if f is **both** O(g) AND Ω(g)

**Think of it as:** a **tight bound** — f and g grow at the same rate (up to a constant).

> **Example from slides:** $f(n) = 3n^2 + 4n + 7$[^1]
> - Θ(n²)? **YES** — it's both O(n²) and Ω(n²) ✅

> 🆕 **Extra Example:** $f(n) = 10n + 50$
> - Θ(n)? **YES** — it's O(n) and Ω(n) ✅
> - Θ(n²)? **NO** — it's O(n²) but NOT Ω(n²) ❌

***

#### Quick Summary Table

| Notation | Meaning | Analogy |
| :-- | :-- | :-- |
| O(g) | f grows **at most** as fast as g | f ≤ g (upper bound) |
| Ω(g) | f grows **at least** as fast as g | f ≥ g (lower bound) |
| Θ(g) | f grows **exactly** as fast as g | f ≈ g (tight bound) |


***

#### How to Check Using Limits

You can determine the relationship between f and g by computing:[^1]

$$
\lim_{n \to \infty} \frac{f(n)}{g(n)}
$$


| Limit Result | Conclusion |
| :-- | :-- |
| = 0 | f grows **slower** than g → f is O(g) but **not** Ω(g) |
| = constant c > 0 | f and g grow at the **same rate** → f is Θ(g) |
| = ∞ | f grows **faster** than g → f is Ω(g) but **not** O(g) |

> **Walkthrough Example:** Is $f(n) = 3n^2 + 4n$ a Θ(n²)?
>

> $$
\lim_{n \to \infty} \frac{3n^2 + 4n}{n^2} = \lim_{n \to \infty} \left(3 + \frac{4}{n}\right) = 3
$$

>
> The limit = 3 (a positive constant) → **YES, it is Θ(n²)** ✅

***

## 📋 Exam Preparation Checklist

Make sure you understand and can explain:

- [ ] The difference between **computability** and **complexity**
- [ ] What a **computational task** and **process (algorithm)** are
- [ ] The three constraints every algorithm must satisfy
- [ ] The **Multiplication Problem** — RepeatedAddition vs GridMethod
- [ ] The **Wedding Problem** and why 2ⁿ subsets is impractical
- [ ] What **P** and **NP** mean (informally)
- [ ] **Strings**, alphabets, concatenation, and string notation
- [ ] **Functions**, relations, and Cartesian products
- [ ] How to **encode** data as binary strings
- [ ] **Languages** and decision problems
- [ ] **Big-O, Ω, and Θ** — definitions, examples, and limit method

<div align="center">⁂</div>

[^1]: preliminaries.pdf

