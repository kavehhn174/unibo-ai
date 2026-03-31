# Statistical and Mathematical Methods for Artificial Intelligence
### Course: 91255 | University of Bologna
### Instructor: Elena Loli Piccolomini | [elena.loli@unibo.it](mailto:elena.loli@unibo.it)

---

## 📖 About This Repository

This repository contains **complete, exam-ready study notes** for the course *Statistical and Mathematical Methods for Artificial Intelligence* (Course code: 91255) at the University of Bologna.

The notes are structured to be **self-contained**: a student should be able to read them without referring back to the original slides. Each chapter includes:
- 📐 Full mathematical derivations with step-by-step solutions
- 💡 Supplementary intuitions and real-world AI connections
- ⚠️ Exam tips highlighting the most likely exam topics
- 🧮 3 extra solved exercises per topic
- 📝 30 review questions with full answers

---

## 📚 Course Chapters

| # | Chapter | Topics Covered | Notes |
|---|---|---|---|
| 1 | [Numerical Computation and Finite Numbers](./finite_numbers.md) | Error types, floating-point systems, IEEE 754, machine epsilon, cancellation | [`finite_numbers.md`](./finite_numbers.md) |
| 2 | [Linear Algebra Prerequisites for AI](./prerequisites.md) | Vector spaces, matrices, determinants, eigenvalues, norms | [`prerequisites.md`](./prerequisites.md) |
| *(more chapters coming)* | | | |

---

## 🗺️ Course Overview

This course bridges **mathematical foundations** and **artificial intelligence**, building the quantitative toolkit needed to understand, implement, and analyze modern AI algorithms.

### Part 1 — Numerical Foundations
The course begins by establishing how real numbers are stored in finite-precision computers. Key concepts include the **floating-point number system** $F(\beta, t, L, U)$, **machine epsilon** $\varepsilon_{\text{mach}}$, **rounding rules**, and the **IEEE 754 standard** (single and double precision). Understanding these foundations is critical because every AI computation runs on finite arithmetic — ignoring it leads to subtle, hard-to-debug numerical errors.

### Part 2 — Linear Algebra
The backbone of virtually every AI/ML algorithm is linear algebra. This part covers **vector spaces** and their structure, **matrix operations** (multiplication, transposition, inversion), **determinants**, **eigenvalues and eigenvectors**, and **norms** (both vector and matrix). PCA, linear regression, neural network layers, and optimization algorithms all rely directly on these concepts.

### Part 3 *(coming)*
Further chapters cover topics such as probability and statistics, optimization, and numerical linear algebra — forming the complete mathematical foundation for AI.

---

## 🔑 Key Formulas at a Glance

### Floating-Point Systems

| Quantity | Formula |
|---|---|
| Total normalized floats | $2(\beta-1)\beta^{t-1}(U-L+1)+1$ |
| Underflow level (UFL) | $\beta^{L-1}$ |
| Overflow level (OFL) | $\beta^U(1-\beta^{-t})$ |
| Machine epsilon (round-to-nearest) | $\varepsilon_{\text{mach}} = \frac{1}{2}\beta^{1-t}$ |
| Relative error bound | $\left|\frac{\text{fl}(x)-x}{x}\right| \leq \varepsilon_{\text{mach}}$ |

### Linear Algebra

| Concept | Key Result |
|---|---|
| Invertibility | $A$ invertible $\iff$ $\det(A)\neq 0$ $\iff$ $\text{rank}(A)=n$ |
| Inverse formula | $A^{-1} = C^T / \det(A)$ |
| Eigenvalues | Solve $\det(A-\lambda I)=0$ |
| Determinant & eigenvalues | $\det(A) = \prod_i \lambda_i$ |
| Spectral radius | $\rho(A) = \max_{\lambda\in\sigma(A)}|\lambda|$ |
| Spectral norm | $\|A\|_2 = \sqrt{\rho(A^TA)}$ |

---

## ⚠️ Top Exam Topics

Based on the material, these are the highest-priority topics for the final exam:

1. **Error types** — name and distinguish all 4 sources of approximation
2. **Absolute vs relative error** — definition and computation
3. **Floating-point system properties** — $N$, UFL, OFL formulas applied to concrete $F(\beta,t,L,U)$
4. **Machine epsilon** — definition (two forms), IEEE values for single/double precision
5. **Rounding rules** — chop vs round-to-nearest, which is IEEE default
6. **Non-associativity** of floating-point arithmetic — concrete example
7. **Cancellation** — what it is, when it happens, why it's dangerous
8. **Linear independence** — definition and testing
9. **Determinant computation** — $2\times2$, Sarrus' rule, Laplace expansion
10. **Invertibility conditions** — all 5 equivalent conditions
11. **Eigenvalue computation** — characteristic polynomial, eigenvectors
12. **Matrix and vector norms** — 1, 2, $\infty$, Frobenius; memory trick for 1-norm vs $\infty$-norm

---

## 🛠️ How to Use These Notes

1. **First read**: Go through each chapter sequentially. Pay attention to `⚠️ Exam Tip` blocks.
2. **Practice**: After each section, try the additional exercises before reading the solutions.
3. **Self-test**: Use the 30 review questions at the end of each chapter to test your understanding.
4. **Quick review**: Before the exam, use the "Key Formulas" table above as a cheat-sheet.

---

*Notes compiled by an AI study assistant. Based on lecture material by Prof. Elena Loli Piccolomini, A.A. 2025/26.*
