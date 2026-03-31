# Numerical Computation and Finite Numbers — Study Notes
### Course: Statistical and Mathematical Methods for AI | University of Bologna
### Instructor: Elena Loli Piccolomini | A.A. 2025/26

---

## Table of Contents
1. [Problem Solving Process](#1-problem-solving-process)
2. [Sources of Approximation](#2-sources-of-approximation)
3. [Absolute and Relative Error](#3-absolute-and-relative-error)
4. [Significant Digits](#4-significant-digits)
5. [Total Error Decomposition](#5-total-error-decomposition)
6. [Representation of Real Numbers in Base β](#6-representation-of-real-numbers-in-base-β)
7. [Floating-Point Number Systems](#7-floating-point-number-systems)
8. [Normalized Floating-Point Numbers](#8-normalized-floating-point-numbers)
9. [Properties of Floating-Point Systems](#9-properties-of-floating-point-systems)
10. [Rounding Rules](#10-rounding-rules)
11. [IEEE Standard](#11-ieee-standard)
12. [Machine Precision (Machine Epsilon)](#12-machine-precision-machine-epsilon)
13. [Exceptional Values: Inf and NaN](#13-exceptional-values-inf-and-nan)
14. [Floating-Point Arithmetic](#14-floating-point-arithmetic)
15. [Overflow and Underflow](#15-overflow-and-underflow)
16. [Cancellation](#16-cancellation)
17. [📝 Review Questions (30 Questions)](#-review-questions-30-questions)

---

## 1. Problem Solving Process

When using a computer to solve real-world problems numerically, the general workflow is:

1. **Develop a mathematical model** of the physical/real problem
2. **Develop algorithms** for the numerical solution of the model
3. **Implement algorithms** in computer software
4. **Run the software** to simulate the process numerically
5. **Visualize the results** graphically
6. **Interpret and validate** the computed results

> 💡 **Additional Context (from assistant):** This pipeline shows why numerical errors matter at every step. Even a tiny floating-point error introduced in step 3 can compound across millions of operations, making the final result in step 6 unreliable. This is why this entire chapter exists.

---

## 2. Sources of Approximation

When working with numerical methods on a computer, errors are unavoidable. They come from four sources:

| Error Type | Cause |
|---|---|
| **Measure errors** | Due to the measuring instrument itself (sensor noise, precision limits) |
| **Arithmetic errors** (Algorithmic) | Propagation of rounding errors of each operation when an algorithm runs on a computer |
| **Truncation errors** | Truncation of an infinite procedure to a finite one (e.g., approximating an infinite series with a finite sum) |
| **Inherent errors** | Finite representation of input data in a computer |

> ⚠️ **Exam Tip:** Be able to **name all four error types** and give an example for each. Distinguishing *algorithmic* from *inherent* error is a common exam question.

> 💡 **Additional Context (from assistant):** Think of truncation error like approximating $e^x = 1 + x + \frac{x^2}{2!} + \cdots$ with only 3 terms. You *truncate* the infinite series. The discarded terms are the truncation error. This is conceptually separate from the rounding that happens in each arithmetic step (algorithmic error).

---

## 3. Absolute and Relative Error

Let $\tilde{x}$ be an approximation of a true value $x$.

> ⚠️ **Exam Tip:** These definitions are fundamental and almost certainly on the exam. Know them cold.

**Absolute Error:**
$$E_x = |\tilde{x} - x|$$

**Relative Error:**
$$R_x = \left|\frac{\tilde{x} - x}{x}\right|, \quad x \neq 0$$

An equivalent way to express the approximation:
$$\tilde{x} = x(1 + R_x)$$

### Examples from the slides

| True value $x$ | Approximation $\tilde{x}$ | Absolute Error $E_x$ | Relative Error $R_x$ |
|---|---|---|---|
| $3.141592$ | $3.14$ | $0.001592$ | $0.000507$ |
| $1 \times 10^6$ | $999996$ | $4$ | $4 \times 10^{-6}$ |
| $1.2 \times 10^{-5}$ | $0.9 \times 10^{-5}$ | $0.3 \times 10^{-5}$ | $0.25$ |

> 💡 **Additional Context (from assistant):** Notice the third row: even though the absolute error ($3 \times 10^{-6}$) is tiny, the relative error is **25%** — that's enormous! This illustrates why **relative error is more meaningful** when comparing approximations of quantities at very different scales. Always prefer relative error for assessing quality.

### 3 Additional Exercises

**Exercise A.** $x = 2.718281$, $\tilde{x} = 2.72$. Compute $E_x$ and $R_x$.
$$E_x = |2.72 - 2.718281| = 0.001719$$
$$R_x = \frac{0.001719}{2.718281} \approx 0.000632$$

**Exercise B.** $x = 5.0 \times 10^3$, $\tilde{x} = 4997$. Compute $E_x$ and $R_x$.
$$E_x = |4997 - 5000| = 3$$
$$R_x = \frac{3}{5000} = 0.0006$$

**Exercise C.** $x = 0.000423$, $\tilde{x} = 0.000420$. Compute $E_x$ and $R_x$.
$$E_x = |0.000420 - 0.000423| = 3 \times 10^{-6}$$
$$R_x = \frac{3 \times 10^{-6}}{4.23 \times 10^{-4}} \approx 0.00709$$

---

## 4. Significant Digits

**Definition:** The number $\tilde{x}$ approximates $x$ to $d$ **significant digits** if $d$ is the largest non-negative integer such that:
$$\left|\frac{x - \tilde{x}}{x}\right| < \frac{10^{1-d}}{2}$$

> ⚠️ **Exam Tip:** This definition is subtle — know how to use it to determine $d$ from a given pair $(x, \tilde{x})$.

### Example from the slides

$x = 3.141592$, $\tilde{x} = 3.14$:
$$R_x = 0.000507 < \frac{10^{1-3}}{2} = 0.5 \times 10^{-2}$$
So $\tilde{x}$ approximates $x$ to $d = 3$ significant digits.

### 3 Additional Exercises

**Exercise A.** $x = 1.41421$, $\tilde{x} = 1.414$. How many significant digits?
$$R_x = \frac{|1.414 - 1.41421|}{1.41421} = \frac{0.00021}{1.41421} \approx 0.0001484$$
Check: $\frac{10^{1-4}}{2} = 5 \times 10^{-4}$. Since $0.0001484 < 5 \times 10^{-4}$, we have $d = 4$ significant digits.

**Exercise B.** $x = 0.577350$, $\tilde{x} = 0.577$. How many significant digits?
$$R_x = \frac{|0.577 - 0.577350|}{0.577350} \approx 0.000606$$
Check $d=3$: $\frac{10^{-2}}{2} = 5 \times 10^{-3}$. Since $0.000606 < 5 \times 10^{-3}$, $d = 3$ ✓
Check $d=4$: $\frac{10^{-3}}{2} = 5 \times 10^{-4}$. Since $0.000606 > 5 \times 10^{-4}$, $d \neq 4$. So $d = 3$.

**Exercise C.** $x = 9.99999$, $\tilde{x} = 10.0$. How many significant digits?
$$R_x = \frac{|10.0 - 9.99999|}{9.99999} = \frac{10^{-5}}{9.99999} \approx 10^{-6}$$
Check $d=6$: $\frac{10^{1-6}}{2} = 5 \times 10^{-6}$. Since $10^{-6} < 5 \times 10^{-6}$, $d = 6$.

---

## 5. Total Error Decomposition

When computing $f(x)$ approximately using $\tilde{f}(\tilde{x})$, the total error splits as:

$$\underbrace{\tilde{f}(\tilde{x}) - f(x)}_{\text{total error}} = \underbrace{\tilde{f}(\tilde{x}) - f(\tilde{x})}_{\text{algorithmic error}} + \underbrace{f(\tilde{x}) - f(x)}_{\text{inherent (data) error}}$$

- **Algorithmic error** $= \tilde{f}(\tilde{x}) - f(\tilde{x})$: caused by approximate computation (rounding during algorithm steps)
- **Inherent error** $= f(\tilde{x}) - f(x)$: caused by imprecise input data; **independent of the algorithm chosen**

> ⚠️ **Exam Tip:** The key insight is that **the choice of algorithm does not affect the inherent (data) error**. The algorithm only controls the algorithmic error.

---

## 6. Representation of Real Numbers in Base β

Given integer $\beta > 1$, any real $x \neq 0$ can be uniquely expressed as:
$$x = \text{sign}(x) \cdot (d_1 \beta^{-1} + d_2 \beta^{-2} + \ldots) \cdot \beta^p = \text{sign}(x) \cdot m \cdot \beta^p$$

where:
- $0 \leq d_i \leq \beta - 1$ for all $i$
- $d_1 \neq 0$ (normalization condition)
- $m$ is the **mantissa**: $\frac{1}{\beta} \leq m < 1$
- $p$ is the **exponent**

**Normalized scientific notation:** $x = \pm (0.d_1 d_2 d_3 \ldots) \cdot \beta^p$

> 💡 **Additional Context (from assistant):** In base 10, this is just standard scientific notation: $3.14159 = 0.314159 \times 10^1$. In base 2 (binary), the leading digit $d_1$ is always 1, which is why it can be stored implicitly (the "hidden bit" trick used in IEEE 754).

**Handwritten note from professor:** Examples of base-10 representation:
- $12.5_{(10)} = (0.125) \times 10^2$, so $m = 0.125$, $p = 2$
- $3.55_{(10)} = (0.355) \times 10^1$ (a terminating decimal — exactly representable)
- Note: $0.\overline{6} = (0.6666\ldots)$ is a repeating decimal, meaning it **cannot** be finitely represented → inherent error

---

## 7. Floating-Point Number Systems

A **floating-point system** $F(\beta, t, L, U)$ is defined by 4 parameters:

| Parameter | Meaning |
|---|---|
| $\beta$ | Base (radix) |
| $t$ | Precision (number of mantissa digits) |
| $L$ | Lower bound of exponent |
| $U$ | Upper bound of exponent |

Any floating-point number $x \in F(\beta, t, L, U)$ has the form:
$$x = \pm (d_1 \beta^{-1} + d_2 \beta^{-2} + \cdots + d_t \beta^{-t}) \cdot \beta^p, \quad L \leq p \leq U$$

where $0 \leq d_i \leq \beta - 1$ for $i = 1, \ldots, t$.

> ⚠️ **Exam Tip:** Know how to interpret the 4 parameters and how they define the representable number set. Problems that ask you to **compute properties of a given $F(\beta, t, L, U)$ system** are very common.

---

## 8. Normalized Floating-Point Numbers

A floating-point system is **normalized** when $d_1 \neq 0$. This means:
$$\frac{1}{\beta} \leq d_1 < 1$$

**Why normalize?**
1. Representation of each number is **unique**
2. **No digits wasted** on leading zeros
3. In binary ($\beta = 2$), the leading bit $d_1$ is always 1 and need **not be stored** ("hidden bit") → saves 1 bit of storage

> 💡 **Additional Context (from assistant):** The hidden bit is why IEEE 754 single precision gives you 24 bits of mantissa precision from only 23 stored bits — the leading 1 is implied.

---

## 9. Properties of Floating-Point Systems

> ⚠️ **Exam Tip:** These formulas are heavily tested. Memorize all of them.

**Total number of normalized floating-point numbers** (including zero):
$$N = 2(\beta - 1)\beta^{t-1}(U - L + 1) + 1$$

**Smallest positive normalized number (Underflow Level):**
$$\text{UFL} = \beta^{L-1}$$

**Largest floating-point number (Overflow Level):**
$$\text{OFL} = \beta^U (1 - \beta^{-t})$$

**Key properties:**
- The system is **finite and discrete** — only a countable set of real numbers is exactly representable
- Floating-point numbers are **equally spaced only between successive powers of $\beta$**; the gap grows as numbers get larger
- Real numbers not in the system must be **rounded** to the nearest representable value

### Worked Example: $F(2, 3, -1, 1)$

$$N = 2(2-1) \cdot 2^{3-1} \cdot (1 - (-1) + 1) + 1 = 2 \cdot 1 \cdot 4 \cdot 3 + 1 = 25$$
$$\text{UFL} = 2^{-1-1} = 2^{-2} = 0.25$$
$$\text{OFL} = 2^1 (1 - 2^{-3}) = 2 \cdot \frac{7}{8} = 1.75$$

### 3 Additional Exercises

**Exercise A.** Compute $N$, UFL, OFL for $F(10, 4, -10, 10)$.
$$N = 2(10-1) \cdot 10^{4-1} \cdot (10 - (-10) + 1) + 1 = 2 \cdot 9 \cdot 1000 \cdot 21 + 1 = 378001$$
$$\text{UFL} = 10^{-10-1} = 10^{-11}$$
$$\text{OFL} = 10^{10}(1 - 10^{-4}) = 10^{10} \cdot 0.9999 = 9.999 \times 10^9$$

**Exercise B.** Compute $N$, UFL, OFL for $F(2, 4, -2, 2)$.
$$N = 2(2-1) \cdot 2^{4-1} \cdot (2 - (-2) + 1) + 1 = 2 \cdot 1 \cdot 8 \cdot 5 + 1 = 81$$
$$\text{UFL} = 2^{-2-1} = 2^{-3} = 0.125$$
$$\text{OFL} = 2^2(1 - 2^{-4}) = 4 \cdot \frac{15}{16} = 3.75$$

**Exercise C.** For $F(\beta, 7, 4, 4)$, how many normalized numbers exist if $\beta = 10$?

> 💡 **Handwritten note from professor:** This is the "$F(\beta, 7, 4, 4)$" system referenced in the slides margins.

$$N = 2(10-1) \cdot 10^{7-1} \cdot (4 - 4 + 1) + 1 = 2 \cdot 9 \cdot 10^6 \cdot 1 + 1 = 18000001$$

---

## 10. Rounding Rules

When a real number $x$ cannot be exactly represented in $F(\beta, t, L, U)$, it is approximated by $\text{fl}(x)$ using a rounding rule:

| Rule | Description |
|---|---|
| **Chop (round toward zero)** | Truncate the base-$\beta$ expansion after $t$ digits |
| **Round to nearest (round to even)** | $\text{fl}(x)$ is the nearest floating-point number; on a tie, choose the one whose last digit is even |

**Round to nearest is the default in IEEE systems** and is more accurate.

### Worked Example from professor's notes: $F(10, 4, -10, 10)$

For $y = \pi = 3.141592\ldots$, represented as $x = (0.3141592\ldots) \times 10^1$:
- **Chop:** $\text{fl}(y) = 0.3141 \times 10^1 = 3.141$
- **Round to nearest:** $\text{fl}(y) = 0.3142 \times 10^1 = 3.142$ (since the 5th digit is 5, round up)

### 3 Additional Exercises

**Exercise A.** In $F(10, 3, -5, 5)$, represent $x = 2.7182818$ using both rules.
- Normalize: $x = (0.27182818) \times 10^1$
- **Chop:** $\text{fl}(x) = 0.271 \times 10^1 = 2.71$
- **Round:** 4th digit is $8 \geq 5$, so $\text{fl}(x) = 0.272 \times 10^1 = 2.72$

**Exercise B.** In $F(2, 3, -4, 4)$, represent $x = 0.1_{10}$ (i.e., $1/10$ in base 2) using chop.

First, convert $0.1_{10}$ to binary:
$0.1 \times 2 = 0.2 \to 0$, $0.2 \times 2 = 0.4 \to 0$, $0.4 \times 2 = 0.8 \to 0$, $0.8 \times 2 = 1.6 \to 1$, $0.6 \times 2 = 1.2 \to 1$, $0.2 \times 2 = \ldots$ (repeating)
$0.1_{10} = 0.0001100110011\ldots_2 = (0.1100110011\ldots)_2 \times 2^{-3}$

**Chop** to $t=3$ digits: $\text{fl}(x) = (0.110)_2 \times 2^{-3} = 0.75 \times 0.125 = 0.09375$

> 💡 **Additional Context (from assistant):** This is a crucial example! $0.1$ is **not exactly representable** in binary — it becomes an infinitely repeating binary fraction. This is why `0.1 + 0.2 != 0.3` in most programming languages.

**Exercise C.** In $F(10, 4, -5, 5)$, apply round-to-nearest to $x = 1.23450$.
Normalized: $(0.12345) \times 10^1$. The 5th digit is 5 (a tie). We look at the 4th digit: 4 (even). Tie-breaking rule says keep 4 (even last digit). So $\text{fl}(x) = 0.1234 \times 10^1 = 1.234$.

---

## 11. IEEE Standard

> ⚠️ **Exam Tip:** Know the bit layout for both single and double precision. This is frequently asked.

The IEEE 754 standard defines two main floating-point formats using $\beta = 2$:

| Format | Total bits | Sign | Mantissa (stored) | Exponent | Total mantissa precision |
|---|---|---|---|---|---|
| **Single Precision** | 32 | 1 | 23 | 8 | 24 (hidden bit) |
| **Double Precision** | 64 | 1 | 52 | 11 | 53 (hidden bit) |

The exponent is stored with a **bias** (e.g., bias = 127 for single precision), so the actual exponent is:
$$e = e_{\text{stored}} - \text{bias}$$

**Example from the lecture notes:**
```
0 01111100 01000000000000000000000  →  0.15625
```
- Sign bit = 0 (positive)
- Stored exponent = $01111100_2 = 124_{10}$, actual exponent $e = 124 - 127 = -3$
- Mantissa (with hidden bit) = $1.01_2$
- Value: $(1.01)_2 \times 2^{-3} = (1 \cdot 2^0 + 0 \cdot 2^{-1} + 1 \cdot 2^{-2}) \times 2^{-3} = 1.25 \times 0.125 = 0.15625$ ✓

> 💡 **Additional Context (from assistant):** In binary, since $d_1$ is always 1 in a normalized number, we don't need to store it — we know it's there. This is the "hidden bit." So 23 stored bits actually give you 24 bits of precision. This is why single precision has about $2^{24} \approx 16.7$ million distinct mantissa values.

---

## 12. Machine Precision (Machine Epsilon)

**Machine epsilon** $\varepsilon_{\text{mach}}$ characterizes the accuracy of a floating-point system.

| Rounding Rule | $\varepsilon_{\text{mach}}$ |
|---|---|
| Chop (round toward zero) | $\beta^{1-t}$ |
| Round to nearest | $\frac{1}{2}\beta^{1-t}$ |

**Alternative definition:** $\varepsilon_{\text{mach}}$ is the smallest positive number such that:
$$\text{fl}(1 + \varepsilon_{\text{mach}}) > 1$$

**Maximum relative error bound:**
$$\left|\frac{\text{fl}(x) - x}{x}\right| \leq \varepsilon_{\text{mach}}$$

> ⚠️ **Exam Tip:** The max relative error bound and the definition of $\varepsilon_{\text{mach}}$ are critical. You may be asked to prove or derive the bound.

### Values for Standard IEEE Systems

| System | $t$ (total mantissa digits) | $\varepsilon_{\text{mach}}$ (round-to-nearest) | Decimal digits |
|---|---|---|---|
| **Single Precision** | $t = 24$ | $2^{-23} \approx 1.19 \times 10^{-7}$ | ~7 digits |
| **Double Precision** | $t = 53$ | $2^{-52} \approx 2.22 \times 10^{-16}$ | ~16 digits |

> 💡 **Handwritten note from professor:** 
> - Single precision: stored mantissa digits = 23, total = 24 (hidden bit), $\varepsilon_{\text{mach}} = 2^{1-24} = 2^{-23} \approx 10^{-7}$
> - Double precision: stored mantissa digits = 52, total = 53, $\varepsilon_{\text{mach}} = 2^{1-53} = 2^{-52} \approx 10^{-16}$

### Example: $F(2, 3, -1, 1)$
- Chop: $\varepsilon_{\text{mach}} = 2^{1-3} = 2^{-2} = 0.25$
- Round to nearest: $\varepsilon_{\text{mach}} = \frac{1}{2} \cdot 2^{-2} = 2^{-3} = 0.125$

### 3 Additional Exercises

**Exercise A.** Compute $\varepsilon_{\text{mach}}$ (round-to-nearest) for $F(10, 6, -10, 10)$.
$$\varepsilon_{\text{mach}} = \frac{1}{2} \cdot 10^{1-6} = \frac{1}{2} \cdot 10^{-5} = 5 \times 10^{-6}$$

**Exercise B.** In $F(2, 8, L, U)$ with round-to-nearest, compute $\varepsilon_{\text{mach}}$.
$$\varepsilon_{\text{mach}} = \frac{1}{2} \cdot 2^{1-8} = \frac{1}{2} \cdot 2^{-7} = 2^{-8} = \frac{1}{256} \approx 0.00391$$

**Exercise C.** A system uses round-by-chop with $\varepsilon_{\text{mach}} = 10^{-6}$. What is the precision $t$ if $\beta = 10$?
$$\varepsilon_{\text{mach}} = \beta^{1-t} \Rightarrow 10^{-6} = 10^{1-t} \Rightarrow 1-t = -6 \Rightarrow t = 7$$

---

## 13. Exceptional Values: Inf and NaN

The IEEE standard reserves special exponent patterns for exceptional cases:

| Special Value | Triggered By | Example |
|---|---|---|
| **Inf** (infinity) | Dividing a finite number by zero | `1/0` |
| **NaN** (Not a Number) | Undefined or indeterminate operations | `0/0`, `0 * Inf`, `Inf/Inf`, $\sqrt{-1}$ |

> 💡 **Additional Context (from assistant):** In Python/NumPy:
> ```python
> import numpy as np
> np.float64(1) / np.float64(0)   # → inf
> np.float64(0) / np.float64(0)   # → nan
> ```
> These are not errors — the program continues running. This can cause silent bugs in numerical code if not checked explicitly.

---

## 14. Floating-Point Arithmetic

Computers operate on **finite representations**, so floating-point arithmetic results differ from exact real arithmetic.

Let $\odot$ denote a floating-point operation and $\cdot$ its real counterpart:
$$x \odot y = \text{fl}(x \cdot y)$$

The relative rounding error of each operation is bounded:
$$\left|\frac{(x \odot y) - (x \cdot y)}{x \cdot y}\right| < \varepsilon_{\text{mach}}$$

**Steps for executing a floating-point operation** $x \oplus y$:
1. Compute the **exact** result $z = x + y$ (in extended precision register)
2. Round $z$ to a floating-point number: $x \oplus y = \text{fl}(z)$

### Issues in floating-point arithmetic:

| Operation | Problem |
|---|---|
| **Addition/Subtraction** | Shifting mantissa to align exponents may lose digits of the smaller number |
| **Multiplication** | Product of two $t$-digit mantissas has up to $2t$ digits, causing truncation |
| **Division** | Quotient may be non-terminating (e.g., $1/10$ in binary) |

> ⚠️ **Exam Tip:** Floating-point **addition and multiplication are commutative but NOT associative**. This is a key and often-tested property.

### Non-associativity Example

In $F(10, 2)$: $x = 0.11$, $y = 0.13 \times 10^{-1}$, $z = 0.14 \times 10^{-1}$:
$$(x + y) + z = 0.13 \times 10^0, \quad x + (y + z) = 0.14 \times 10^0$$

Different results! The order of operations matters in floating-point.

### Worked Example: $F(10, 6, -10, 10)$

$x = 192.403$, $y = 0.635782$:
- $\text{fl}(x) = 0.192403 \times 10^3$, $\text{fl}(y) = 0.635782 \times 10^0$
- $\text{fl}(x) + \text{fl}(y) = (0.192403 + 0.000635782) \times 10^3 = 0.193038782 \times 10^3$
- $\text{fl}(z) = 0.193039 \times 10^3$ (last two digits of $y$ are lost)
- $\text{fl}(x) \times \text{fl}(y) = 0.122326364146 \times 10^3 \Rightarrow \text{fl}(w) = 0.122326 \times 10^3$ (half the product digits lost)

### 3 Additional Exercises

**Exercise A.** In $F(10, 4, -10, 10)$: $x = 1234.5$, $y = 0.00678$. Compute $\text{fl}(x + y)$.
- $\text{fl}(x) = 0.1235 \times 10^4$, $\text{fl}(y) = 0.6780 \times 10^{-2}$
- Align: $y$ in terms of $10^4$: $0.000000678 \times 10^4$
- Sum: $0.123500678 \times 10^4 \Rightarrow \text{fl} = 0.1235 \times 10^4$ (y has no effect on the result!)

**Exercise B.** In $F(10, 3, -5, 5)$: $x = 100$, $y = 0.123$, $z = 0.234$. Verify non-associativity.
- $(x \oplus y) \oplus z$: $x + y = 100.123 \Rightarrow \text{fl} = 100$; then $100 + 0.234 = 100.234 \Rightarrow \text{fl} = 100$
- $x \oplus (y \oplus z)$: $y + z = 0.357 \Rightarrow \text{fl} = 0.357$; then $100 + 0.357 = 100.357 \Rightarrow \text{fl} = 100$
- Same here, but try $x = 0.11 \times 10^2$, $y = 0.13 \times 10^{-3}$, $z = 0.14 \times 10^{-3}$ for clear non-associativity.

**Exercise C.** Why is $1/10$ problematic in binary floating-point?

$1/10 = 0.0001100110011\ldots_2$ — a non-terminating binary fraction. No finite precision binary system can represent it exactly, so every time $0.1$ is stored, there's an inherent error. This is why `0.1 + 0.2 - 0.3` in Python gives approximately `5.5 \times 10^{-17}` instead of 0.

---

## 15. Overflow and Underflow

When the result of a floating-point operation has an exponent outside $[L, U]$:

| Situation | Condition | Consequence |
|---|---|---|
| **Overflow** | $p > U$ | Result too large; usually **fatal** — no good approximation exists |
| **Underflow** | $p < L$ | Result too small; often silently **set to zero** (reasonable approximation) |

> 💡 **Handwritten note from professor:** Overflow is usually more serious than underflow. There is no good approximation for arbitrarily large magnitudes, while **zero is often a reasonable approximation for arbitrarily small magnitudes**.

> ⚠️ **Exam Tip:** Distinguish overflow vs. underflow and explain why overflow is more dangerous.

---

## 16. Cancellation

**Cancellation** occurs when subtracting two numbers of similar magnitude and the same sign.

The leading digits cancel, leaving a result with **fewer significant digits**.

### Example from the slides

$x = 1.92403 \times 10^2$, $y = 1.92275 \times 10^2$:
$$z = \text{fl}(x - y) = (0.192403 - 0.192275) \times 10^3 = 0.000128 \times 10^3$$
$$\text{fl}(z) = 0.128000 \times 10^3$$

The result $128$ is exactly representable — but it has only **3 significant digits** instead of 6.

> ⚠️ **Exam Tip:** The two floating-point operations that produce the **greatest relative errors** are:
> 1. **Addition of numbers with very different exponents** — the smaller number loses digits
> 2. **Subtraction of nearly equal numbers (cancellation)** — significant digits are lost

> 💡 **Additional Context (from assistant):** Cancellation is not always avoidable, but it can sometimes be mitigated by algebraic reformulation. For example, $\sqrt{x+1} - \sqrt{x}$ suffers from cancellation for large $x$; it's better computed as $\frac{1}{\sqrt{x+1} + \sqrt{x}}$.

### 3 Additional Exercises

**Exercise A.** $x = 3.14159 \times 10^1$, $y = 3.14152 \times 10^1$ in $F(10, 6)$. Compute $x - y$ and count significant digits.
$$z = (3.14159 - 3.14152) \times 10^1 = 0.00007 \times 10^1 = 0.0007$$
Result has only **1 significant digit**. Severe cancellation.

**Exercise B.** Explain why $f(x) = \sqrt{x+1} - \sqrt{x}$ for $x = 10^8$ suffers cancellation and give the stable alternative.
- Direct: $\sqrt{10^8 + 1} \approx 10^4$, $\sqrt{10^8} = 10^4$. Both round to $10000.0$ — difference is catastrophically inaccurate.
- Stable form: $\frac{1}{\sqrt{x+1} + \sqrt{x}} = \frac{1}{\approx 2 \times 10^4} = 5 \times 10^{-5}$. ✓

**Exercise C.** In $F(10, 4, -5, 5)$: $a = 1.002 \times 10^0$, $b = 1.001 \times 10^0$. How many significant digits in $a - b$?
$$z = 1.002 - 1.001 = 0.001 = 1.000 \times 10^{-3}$$
Only **1 significant digit** out of 4 original digits. Three digits were lost to cancellation.

---

## 📝 Review Questions (30 Questions)

---

### Foundational

**Q1. ⚠️ What are the four sources of approximation error in numerical computation? Give one example of each.**

**Answer:**
1. **Measure errors** — e.g., a thermometer reading $23.1°C$ instead of the true $23.127°C$
2. **Algorithmic errors** — rounding errors accumulated as each arithmetic step is executed on a computer
3. **Truncation errors** — computing $e^x \approx 1 + x + x^2/2!$ using only 3 terms
4. **Inherent errors** — storing $1/3 = 0.333...$ in a finite-precision system

---

**Q2. ⚠️ Define absolute error and relative error. When is each more appropriate?**

**Answer:**
- Absolute error: $E_x = |\tilde{x} - x|$
- Relative error: $R_x = \frac{|\tilde{x} - x|}{|x|}$, $x \neq 0$

Absolute error measures the raw deviation. Relative error measures it as a fraction of the true value — more meaningful when comparing quantities at different scales. Use relative error for assessing quality of approximation.

---

**Q3. Compute $E_x$ and $R_x$ for $x = 1000000$, $\tilde{x} = 999996$.**

**Answer:**
$$E_x = |999996 - 1000000| = 4$$
$$R_x = \frac{4}{1000000} = 4 \times 10^{-6}$$

Note: large absolute error, tiny relative error. The approximation is very good in relative terms.

---

**Q4. ⚠️ What does it mean for $\tilde{x}$ to approximate $x$ to $d$ significant digits? State the formal definition.**

**Answer:**
$\tilde{x}$ approximates $x$ to $d$ significant digits if $d$ is the largest non-negative integer such that:
$$\left|\frac{x - \tilde{x}}{x}\right| < \frac{10^{1-d}}{2}$$

---

**Q5. How many significant digits does $\tilde{x} = 2.72$ have for $x = 2.71828$?**

**Answer:**
$$R_x = \frac{|2.72 - 2.71828|}{2.71828} = \frac{0.00172}{2.71828} \approx 6.33 \times 10^{-4}$$
Check $d=3$: $\frac{10^{1-3}}{2} = 5 \times 10^{-3}$. Since $6.33 \times 10^{-4} < 5 \times 10^{-3}$: ✓
Check $d=4$: $\frac{10^{-3}}{2} = 5 \times 10^{-4}$. Since $6.33 \times 10^{-4} > 5 \times 10^{-4}$: ✗

$\tilde{x}$ approximates $x$ to **3 significant digits**.

---

**Q6. Write the total error decomposition formula and explain each term.**

**Answer:**
$$\tilde{f}(\tilde{x}) - f(x) = \underbrace{[\tilde{f}(\tilde{x}) - f(\tilde{x})]}_{\text{algorithmic error}} + \underbrace{[f(\tilde{x}) - f(x)]}_{\text{inherent error}}$$

- Algorithmic error: error from using an approximate algorithm $\tilde{f}$ instead of exact $f$
- Inherent error: error from using approximate input $\tilde{x}$ instead of exact $x$
- **Key point:** inherent error is independent of the algorithm — changing the algorithm cannot fix it.

---

**Q7. Express $x = 12.5$ in normalized scientific notation for base $\beta = 10$.**

**Answer:**
$$12.5 = (0.125) \times 10^2$$
Mantissa $m = 0.125$, exponent $p = 2$. ✓

---

**Q8. What are the 4 parameters that define a floating-point system? What does each control?**

**Answer:**
- $\beta$ (base): which number system (2 = binary, 10 = decimal)
- $t$ (precision): how many mantissa digits are stored → controls accuracy
- $L$ (lower exponent bound): controls smallest representable magnitude
- $U$ (upper exponent bound): controls largest representable magnitude

---

**Q9. ⚠️ For $F(\beta, t, L, U)$, write the formulas for: (a) total number of normalized floats, (b) UFL, (c) OFL.**

**Answer:**
- (a) $N = 2(\beta - 1)\beta^{t-1}(U - L + 1) + 1$
- (b) $\text{UFL} = \beta^{L-1}$
- (c) $\text{OFL} = \beta^U(1 - \beta^{-t})$

---

**Q10. Compute all properties of $F(2, 3, -1, 1)$: count, UFL, OFL.**

**Answer:**
$$N = 2(2-1) \cdot 2^2 \cdot (1-(-1)+1) + 1 = 2 \cdot 4 \cdot 3 + 1 = 25$$
$$\text{UFL} = 2^{-1-1} = 2^{-2} = 0.25$$
$$\text{OFL} = 2^1(1 - 2^{-3}) = 2 \times \frac{7}{8} = 1.75$$

---

**Q11. Why are floating-point numbers NOT equally spaced across their entire range?**

**Answer:**
Between successive powers of $\beta$, the gap between consecutive floats is constant. However, this gap *doubles* each time you move to the next power of $\beta$. Numbers near zero are very densely packed; numbers near OFL are very sparse. This is a fundamental property of the representation.

---

**Q12. What is normalized representation? Why is it used?**

**Answer:**
A normalized floating-point number has $d_1 \neq 0$ (leading digit is non-zero). Benefits:
1. Unique representation (no ambiguity like $0.01 \times 10^3 = 0.1 \times 10^2$)
2. No wasted digits on leading zeros
3. In binary, the leading 1 is implicit (hidden bit), giving one extra bit of precision for free

---

**Q13. ⚠️ Describe the two rounding rules: chop and round-to-nearest. Which is default in IEEE?**

**Answer:**
- **Chop:** Truncate the base-$\beta$ expansion after $t$ digits. Simple but less accurate.
- **Round to nearest:** Choose the closest floating-point number. On ties, choose the one with an even last digit (round-to-even). More accurate.

**IEEE default is round-to-nearest** (round-to-even on ties).

---

**Q14. Apply both rounding rules to represent $\pi = 3.14159265...$ in $F(10, 5, -10, 10)$.**

**Answer:**
Normalized: $(0.314159265...) \times 10^1$
- **Chop** (keep 5 digits): $\text{fl}(\pi) = 0.31415 \times 10^1 = 3.1415$
- **Round to nearest** (6th digit is 9 ≥ 5, round up): $\text{fl}(\pi) = 0.31416 \times 10^1 = 3.1416$

---

**Q15. ⚠️ Draw the IEEE 754 single and double precision word layouts.**

**Answer:**

**Single Precision (32 bits):**
```
[1 sign][8 exponent][23 mantissa] = 32 bits
```

**Double Precision (64 bits):**
```
[1 sign][11 exponent][52 mantissa] = 64 bits
```

In both cases, the first mantissa bit (always 1 due to normalization) is the hidden bit and is not stored.

---

**Q16. Decode the 32-bit IEEE float: `0 01111110 10000000000000000000000`.**

**Answer:**
- Sign = 0 → positive
- Stored exponent = $01111110_2 = 126_{10}$, actual $e = 126 - 127 = -1$
- Mantissa (with hidden bit) = $1.1_2 = 1.5_{10}$
- Value = $1.5 \times 2^{-1} = 0.75$ ✓

---

**Q17. ⚠️ Define machine epsilon two ways. What is $\varepsilon_{\text{mach}}$ for IEEE single and double precision?**

**Answer:**
1. **Formula:** $\varepsilon_{\text{mach}} = \frac{1}{2}\beta^{1-t}$ (round-to-nearest)
2. **Alternative:** Smallest $\varepsilon > 0$ such that $\text{fl}(1 + \varepsilon) > 1$

| System | $\varepsilon_{\text{mach}}$ |
|---|---|
| Single precision | $2^{-23} \approx 1.19 \times 10^{-7}$ |
| Double precision | $2^{-52} \approx 2.22 \times 10^{-16}$ |

---

**Q18. Compute $\varepsilon_{\text{mach}}$ for $F(2, 3, -1, 1)$ using both rounding rules.**

**Answer:**
- Chop: $\varepsilon_{\text{mach}} = 2^{1-3} = 2^{-2} = 0.25$
- Round to nearest: $\varepsilon_{\text{mach}} = \frac{1}{2} \cdot 2^{1-3} = 2^{-3} = 0.125$

---

**Q19. State and explain the maximum relative error proposition for floating-point systems.**

**Answer:**
**Proposition:** For any real $x$ within the range of the floating-point system:
$$\left|\frac{\text{fl}(x) - x}{x}\right| \leq \varepsilon_{\text{mach}}$$

This means every representable real number is approximated with a relative error no greater than $\varepsilon_{\text{mach}}$. This is the fundamental accuracy guarantee of the floating-point system.

---

**Q20. What are Inf and NaN in IEEE arithmetic? Give an example that produces each.**

**Answer:**
- **Inf**: Result of dividing a finite number by zero. Example: `1.0 / 0.0 = Inf`
- **NaN**: Result of an undefined/indeterminate operation. Example: `0.0 / 0.0 = NaN`, `Inf - Inf = NaN`

Both are encoded via a special reserved exponent pattern in IEEE 754.

---

**Q21. ⚠️ Why is floating-point addition NOT associative? Give a concrete example.**

**Answer:**
In $F(10, 2)$ with $x = 0.11$, $y = 0.13 \times 10^{-1}$, $z = 0.14 \times 10^{-1}$:
- $(x \oplus y) \oplus z = 0.13 \times 10^0$
- $x \oplus (y \oplus z) = 0.14 \times 10^0$

Different results because intermediate rounding changes the outcome. This violates the associative law of real arithmetic and is critical for numerical algorithm design.

---

**Q22. In $F(10, 6, -10, 10)$, compute $\text{fl}(x + y)$ for $x = 192.403$, $y = 0.635782$.**

**Answer:**
$\text{fl}(x) = 0.192403 \times 10^3$, $\text{fl}(y) = 0.635782 \times 10^0 = 0.000635782 \times 10^3$

Exact sum: $(0.192403 + 0.000635782) \times 10^3 = 0.193038782 \times 10^3$

Round to 6 digits: $\text{fl}(z) = 0.193039 \times 10^3 = 193.039$

The last two digits of $y$ ($82$) had no effect. ✓

---

**Q23. ⚠️ Compare overflow and underflow. Which is more dangerous and why?**

**Answer:**
- **Overflow** ($p > U$): result exceeds the largest representable number. There is no good approximation for arbitrarily large numbers → usually **fatal** error.
- **Underflow** ($p < L$): result is smaller than the smallest representable number. Zero is often a reasonable approximation for very small numbers → usually **silently set to zero**.

Overflow is more dangerous because it produces garbage, whereas underflow often has a minimal effect on the computation.

---

**Q24. ⚠️ What is cancellation? When does it occur and why is it a problem?**

**Answer:**
Cancellation occurs when subtracting two floating-point numbers of similar magnitude and the same sign. The leading digits cancel to zero, and the result is dominated by the low-order digits — which have accumulated rounding error. The result is exactly representable but has **far fewer significant digits** than expected.

---

**Q25. Demonstrate cancellation: $x = 3.14159 \times 10^0$, $y = 3.14152 \times 10^0$ in $F(10, 6)$.**

**Answer:**
$$z = x - y = (3.14159 - 3.14152) = 0.00007 = 7.00000 \times 10^{-5}$$
The result has only **1 significant digit** out of the original 6. Five significant digits were lost.

---

**Q26. Name the two floating-point operations that produce the greatest relative errors.**

**Answer:**
As highlighted (handwritten note from professor in the slides):
1. **Addition/subtraction of numbers with very different exponents** — the smaller number's low-order digits are shifted away and lost
2. **Subtraction of nearly equal numbers (cancellation)** — leading digits cancel and significant precision is lost

---

**Q27. ⚠️ Why is $0.1$ problematic in binary floating-point systems?**

**Answer:**
$0.1_{10} = 0.0001100110011\ldots_2$ — a non-terminating binary fraction. It cannot be exactly represented in any binary floating-point system with finite precision $t$. It is always stored as an approximation, introducing an inherent error of approximately $\varepsilon_{\text{mach}}$ relative to the true value. This is why `0.1 + 0.2 != 0.3` in languages like Python, C, and Java.

---

**Q28. A system has $\varepsilon_{\text{mach}} = 10^{-7}$ using chop, with $\beta = 10$. What is $t$?**

**Answer:**
$$\varepsilon_{\text{mach}} = \beta^{1-t} \Rightarrow 10^{-7} = 10^{1-t} \Rightarrow 1 - t = -7 \Rightarrow t = 8$$

---

**Q29. For $F(2, 4, -2, 2)$, compute: (a) number of elements, (b) UFL, (c) OFL, (d) $\varepsilon_{\text{mach}}$ with round-to-nearest.**

**Answer:**
(a) $N = 2(2-1) \cdot 2^3 \cdot (2-(-2)+1) + 1 = 2 \cdot 8 \cdot 5 + 1 = 81$
(b) $\text{UFL} = 2^{-2-1} = 2^{-3} = 0.125$
(c) $\text{OFL} = 2^2(1 - 2^{-4}) = 4 \cdot \frac{15}{16} = 3.75$
(d) $\varepsilon_{\text{mach}} = \frac{1}{2} \cdot 2^{1-4} = \frac{1}{2} \cdot 2^{-3} = 2^{-4} = 0.0625$

---

**Q30. ⚠️ Summarize: what are the fundamental limitations of finite computer arithmetic?**

**Answer:**
1. **Finite set of representable numbers** — real numbers form a continuum; machine numbers are discrete
2. **Rounding on every operation** — each floating-point op introduces error up to $\varepsilon_{\text{mach}}$
3. **Non-associativity** — reordering operations changes results
4. **Overflow and underflow** — extreme values are not representable
5. **Cancellation** — loss of significant digits when subtracting nearly equal numbers
6. **Non-exact constants** — many common numbers ($0.1$, $\pi$, $\sqrt{2}$) cannot be finitely represented

Understanding these limitations is essential for designing reliable numerical algorithms.
