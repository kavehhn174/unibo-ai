# Satisfiability Modulo Theories (SMT) — Study Notes
### Course: Combinatorial Decision Making and Optimization | University of Bologna
### Module 2 | Prof. Roberto Amadini | A.Y. 2025/26

---

## Table of Contents
1. [From SAT to SMT](#1-from-sat-to-smt)
2. [SMT History and Context](#2-smt-history-and-context)
3. [SMT vs SAT vs CP](#3-smt-vs-sat-vs-cp)
4. [Dynamic Symbolic Execution (DSE)](#4-dynamic-symbolic-execution-dse)
5. [FOL Preliminaries: Signatures, Terms, and Formulas](#5-fol-preliminaries-signatures-terms-and-formulas)
6. [Semantics, Models, and Satisfiability](#6-semantics-models-and-satisfiability)
7. [FOL Theories vs SMT Theories](#7-fol-theories-vs-smt-theories)
8. [Expansion and the Ground Satisfiability Problem](#8-expansion-and-the-ground-satisfiability-problem)
9. [Axiomatic Theory Definitions](#9-axiomatic-theory-definitions)
10. [Theories of Interest](#10-theories-of-interest)
    - [EUF — Equality with Uninterpreted Functions](#101-euf--equality-with-uninterpreted-functions)
    - [Linear Arithmetic](#102-linear-arithmetic)
    - [Nonlinear Arithmetic and Floating Points](#103-nonlinear-arithmetic-and-floating-points)
    - [Theory of Bit-Vectors](#104-theory-of-bit-vectors)
    - [Theory of Arrays](#105-theory-of-arrays)
    - [Theory of Strings](#106-theory-of-strings)
11. [SMT-LIB Encoding](#11-smt-lib-encoding)
12. [Take-Home Messages](#12-take-home-messages)
13. [📝 Review Questions (30 Questions)](#-review-questions-30-questions)

---

## 1. From SAT to SMT

**Boolean Satisfiability (SAT)** is the problem of determining whether a propositional logic formula can be made true. It was the first problem proven **NP-complete** (Cook-Levin theorem, 1971) and has spawned enormous research activity.

**Propositional Logic basics:**
- **Atoms** (atomic propositions) are facts that can be `true` or `false`.
- Atoms are combined using **connectives**: $\neg$ (NOT), $\wedge$ (AND), $\vee$ (OR), $\rightarrow$ (implication), $\leftrightarrow$ (biconditional).
- It is **decidable** and can be solved relatively efficiently in practice, but has **limited expressiveness**.

**First-Order Logic (FOL)** is more expressive. FOL allows quantifiers ($\forall$, $\exists$) and predicates over a domain. Example:
$$
(\forall x)(x < u \vee \neg(\exists y)(f(x,y) = u))
$$

### Why not just use full FOL satisfiability?

Many real-world problems **do not need the full power of FOL**. They only need satisfiability *relative to a fixed background theory*. Consider:
$$
y = x + 1 \wedge x < z \wedge z < y
$$
This is typically checked under an arithmetical theory — but *which* one? Integer arithmetic, real arithmetic, or even string theory (where `+` is concatenation) yield completely different answers.

> ⚠️ **Exam Tip:** The key motivation for SMT is that fixing a background theory upfront allows building **more specialized and efficient solvers** — especially for quantifier-free formulas.

**Satisfiability Modulo Theories (SMT)** studies the satisfiability of FOL formulas **with respect to fixed background theories**, such as:
- Theory of integer / real arithmetic
- Arrays
- Strings
- (Multi-)sets, trees, bit-vectors, ...

Some FOL theories are **undecidable**, but we can restrict to **decidable fragments** (sub-theories). SMT solvers such as **Z3** and **CVC5** combine decision procedures over different background theories.

> 💡 **Additional Context (from assistant):** Think of SMT like a "smart" SAT solver that understands math. While SAT only sees true/false variables, SMT understands that $x + 1 = 5$ implies $x = 4$ under integer arithmetic. The "modulo" in SMT means "relative to" or "with respect to" a theory.

---

## 2. SMT History and Context

- **Late 1970s – early 1980s:** Roots of SMT in work by Nelson, Oppen, Shostak, Boyer, and Moore.
- **1990s – 2000s:** Modern SMT research begins; rapid development in theory and practice.
- **2000s – present:** SMT integrated into tools for theorem proving, program analysis, and testing — riding on the wave of SAT solver progress.

Key SMT solvers: **Z3** (Microsoft Research), **CVC5** (formerly CVC4, CVC3).

---

## 3. SMT vs SAT vs CP

| Feature | SAT | CP (Constraint Programming) | SMT |
|---|---|---|---|
| Logic basis | Propositional | Domain-specific constraints | FOL (quantifier-free) |
| Domain type | Boolean | Finite / bounded | Can handle **infinite domains** |
| Reasoning | Clause learning, DPLL | Propagators | SAT abstractions + theory solvers |
| Typical applications | Theorem proving, planning | Scheduling, resource allocation | Program analysis, SW verification |
| Handles nogoods? | Natively | Not always | Natively |

> ⚠️ **Exam Tip:** SMT tackles combinatorial problems from an **orthogonal perspective** to CP. Like CP, it uses domain-specific reasoning; unlike CP, it naturally handles infinite domains. Unlike SAT, it has expressive theory reasoning.

---

## 4. Dynamic Symbolic Execution (DSE)

**Dynamic Symbolic Execution (DSE)** (also called *concolic execution*) runs a program with **concrete inputs** while simultaneously tracking **symbolic expressions** for those inputs. This enables automated test generation and bug detection.

### JavaScript DSE Example

Consider this JavaScript snippet:
```javascript
var x = symStr(); // x is symbolic
var y = "length";
if (x[y] >= 2)       // branch: x.length >= 2
  console.log("PC1")
else if (y[x] === "g") // branch: "length"[x] === "g"
  console.log("PC2")
else
  console.log("PC3")
```

Starting with input `x = ""`:

1. **Execution path:** Falls through to PC3 (line 8)
2. **Path Condition (PC):** $\{\neg(|x| \geq 2),\ \neg(\text{"length"}[x] = \text{"g"})\}$
3. **Negate** $\neg(|x| \geq 2)$ to get $PC' = \{|x| \geq 2,\ \neg(\text{"length"}[x] = \text{"g"})\}$
4. Solve $PC'$: feasible solution $x \leftarrow \text{"aa"}$ → reaches PC1
5. **Negate** $\neg(\text{"length"}[x] = \text{"g"})$ to get $PC'' = \{\neg(|x| \geq 2),\ \text{"length"}[x] = \text{"g"}\}$
6. Solve $PC''$: feasible solution $x \leftarrow \text{"3"}$ (since `"length"[3] === "g"`) → reaches PC2

Final test suite $\{x \leftarrow "",\ x \leftarrow \text{"aa"},\ x \leftarrow \text{"3"}\}$ covers **all branches**.

> 💡 **Additional Context (from assistant):** DSE is a powerful technique for finding bugs. A string solver is needed here because PC conditions involve string length, indexing, and equality — all constructs from the theory of strings. This is a concrete motivation for why SMT with string theories is valuable.

### SMT and Program Analysis
- Faithfully modelling language semantics is hard but **not strictly necessary** — difficult constructs can be approximated.
- SMT solvers offer **stronger reasoning** than SAT, CP, or MIP solvers for software analysis due to FOL over multiple background theories.

![DSE JavaScript Example](images/slide_07_figure_01_dse_js_example.png)
> 📌 **Note:** This image is located on **Slide 7** of the provided slides (figure title: "Example: DSE of JavaScript"). Please place the file as `images/slide_07_figure_01_dse_js_example.png` in the same GitHub repository — it will load automatically once added.

---

## 5. FOL Preliminaries: Signatures, Terms, and Formulas

> ⚠️ **Exam Tip:** This entire section is dense and highly examinable. Know how to construct terms and formulas given a signature.

### Signature

The **signature** of a FOL is $\Sigma = \Sigma^F \cup \Sigma^P$ where:
- $\Sigma^F$ = set of **function symbols** (including constants)
- $\Sigma^P$ = set of **predicate symbols** (including propositions)
- $\Sigma^F_k$ = functions of **arity** $k$ (taking $k$ arguments)
- $\Sigma^P_k$ = predicates of arity $k$
- $\Sigma^F_0$ = **constant symbols** (0-ary functions)
- $\Sigma^P_0$ = **propositional symbols** (0-ary predicates)

In propositional logic: $\Sigma^F = \emptyset$ and $\Sigma^P = \Sigma^P_0 \supseteq \{\bot, \top\}$.

**Focus:** Quantifier-free FOL fragments (no $\exists$, $\forall$) **with equality**. All variables are free (treated as uninterpreted constants).

### Terms

The set $T_\Sigma$ of **terms** is defined inductively:
- $c \in \Sigma^F_0 \Rightarrow c \in T_\Sigma$ (constants are terms)
- $f \in \Sigma^F_k$ and $t_1, \ldots, t_k \in T_\Sigma \Rightarrow f(t_1, \ldots, t_k) \in T_\Sigma$ (function application)
- $\varphi \in F_\Sigma$ and $t_1, t_2 \in T_\Sigma \Rightarrow \text{ite}(\varphi, t_1, t_2) \in T_\Sigma$ (if-then-else)

### Formulas

The set $F_\Sigma$ of **formulas** is defined inductively:
- $\bot, \top \in F_\Sigma$
- $t_1, t_2 \in T_\Sigma \Rightarrow t_1 = t_2 \in F_\Sigma$ (equality atom)
- $A \in \Sigma^P_0 \Rightarrow A \in F_\Sigma$ (propositional atom)
- $p \in \Sigma^P_k$ and $t_1, \ldots, t_k \in T_\Sigma \Rightarrow p(t_1, \ldots, t_k) \in F_\Sigma$ (atomic formula)
- $\varphi \in F_\Sigma \Rightarrow \neg\varphi \in F_\Sigma$
- $\varphi_1, \varphi_2 \in F_\Sigma \Rightarrow \varphi_1 \rightarrow \varphi_2,\ \varphi_1 \leftrightarrow \varphi_2,\ \varphi_1 \wedge \varphi_2,\ \varphi_1 \vee \varphi_2 \in F_\Sigma$

### Literals and Clauses

- **Atom:** An atomic formula.
- **Literal:** An atom (positive literal) or its negation (negative literal).
- **Clause:** A disjunction $\ell_1 \vee \cdots \vee \ell_k$ of literals.
- **Unit clause:** A clause consisting of a single literal $\neq \bot, \top$.
- **CNF (Conjunctive Normal Form):** A conjunction $C_1 \wedge \cdots \wedge C_k$ of clauses.
  - Also written as $\{C_1, \ldots, C_k\}$ or just $C_1, \ldots, C_k$.

### Worked Example

Given:
- **Functions:** `height/1`, `mario/0`, `luigi/0`
- **Predicates:** `tallerThan/2`, `>/2`

Then:
- **Terms:** `mario`, `luigi`, `ite(tallerThan(mario, luigi), mario, luigi)`
- **Formulas:** `tallerThan(x, y) ⟺ height(x) > height(y)`
- **Literals:** `¬tallerThan(mario, luigi)`, `mario > luigi`
- **Clause:** `tallerThan(mario, z) ∨ tallerThan(z, mario)`

> 💡 **Additional Context (from assistant):** The `ite(φ, t1, t2)` term is a **term-level if-then-else**. It returns $t_1$ if $\varphi$ is true, and $t_2$ otherwise. This is important because it allows conditional expressions inside terms — it bridges the formula world and the term world.

---

## 6. Semantics, Models, and Satisfiability

### Models

A **$\Sigma$-model** is a pair $\mathcal{M} = \langle U, (\cdot)^\mathcal{M} \rangle$ where:
- $U$ is the **universe** (non-empty domain)
- $(\cdot)^\mathcal{M}$ is a **mapping** such that:
  - For each $k$-ary function $f \in \Sigma^F_k$: $f^\mathcal{M} : U^k \rightarrow U$
  - For each constant $c \in \Sigma^F_0$: $c^\mathcal{M} \in U$
  - For each $k$-ary predicate $p \in \Sigma^P_k$: $p^\mathcal{M} : U^k \rightarrow \{\text{true}, \text{false}\}$
  - For each proposition $B \in \Sigma^P_0$: $B^\mathcal{M} \in \{\text{true}, \text{false}\}$

### Interpretation

The interpretation $(\cdot)^\mathcal{M}$ extends to all terms and formulas:
- $\bot^\mathcal{M} = \text{false}$, $\top^\mathcal{M} = \text{true}$
- $(t_1 = t_2)^\mathcal{M} = \text{true} \Leftrightarrow t_1^\mathcal{M} = t_2^\mathcal{M}$
- $p(t_1,\ldots,t_k)^\mathcal{M} = p^\mathcal{M}(t_1^\mathcal{M},\ldots,t_k^\mathcal{M})$
- $f(t_1,\ldots,t_k)^\mathcal{M} = f^\mathcal{M}(t_1^\mathcal{M},\ldots,t_k^\mathcal{M})$
- $\text{ite}(\varphi, t_1, t_2)^\mathcal{M} = t_1^\mathcal{M}$ if $\varphi^\mathcal{M} = \text{true}$, else $t_2^\mathcal{M}$

### Sports Model Example

Suppose $\Sigma$ is defined with $\Sigma^F_0 = \{a, b, c, d\}$, $\Sigma^F_2 = \{f, g\}$, $\Sigma^P_1 = \{p\}$, and model $\mathcal{M}$:
- $a^\mathcal{M} = \text{football}$, $b^\mathcal{M} = \text{rugby}$, $c^\mathcal{M} = \text{skiing}$, $d^\mathcal{M} = \text{curling}$
- $f^\mathcal{M}(x, y)$ = sport with more team players between $x$ and $y$
- $g^\mathcal{M}(x, y)$ = globally more popular sport between $x$ and $y$
- $p^\mathcal{M}(x)$ = is $x$ played with a ball?

Interpreting $p(f(f(c, b), g(a, d)))$:
- $f(c, b) = f(\text{skiing}, \text{rugby}) = \text{rugby}$ (more players)
- $g(a, d) = g(\text{football}, \text{curling}) = \text{football}$ (more popular)
- $f(\text{rugby}, \text{football}) = \text{football}$ (more players)
- $p(\text{football}) = \text{true}$ (played with a ball)

### Satisfiability Definitions

> ⚠️ **Exam Tip:** These definitions are fundamental and often appear verbatim in exam questions.

- $\mathcal{M}$ **satisfies** $\varphi$ if $\varphi^\mathcal{M} = \text{true}$ (written $\mathcal{M} \models \varphi$)
- $\mathcal{M}$ **falsifies** $\varphi$ if $\varphi^\mathcal{M} = \text{false}$
- A **$\Sigma$-theory** is a (possibly infinite) set $T$ of $\Sigma$-models
- $\varphi$ is **$T$-satisfiable** if there exists $\mathcal{M} \in T$ satisfying $\varphi$
- $\{\varphi_1, \ldots, \varphi_k\}$ is **$T$-consistent** iff $\varphi_1 \wedge \cdots \wedge \varphi_k$ is $T$-satisfiable
- $\Gamma \subseteq F_\Sigma$ **$T$-entails** $\varphi$ (written $\Gamma \models_T \varphi$) iff every $\mathcal{M} \in T$ satisfying $\Gamma$ also satisfies $\varphi$

**Using the sports model $T = \{\mathcal{M}\}$:**
- `f(a, c) = a` is satisfiable ✓ (football has more players than skiing)
- `p(d)` is unsatisfiable ✗ (curling is not played with a ball)
- `{p(a), p(b), ¬p(c)}` is consistent ✓
- `{g(x,y)=y, g(y,z)=z} ⊨ g(x,z)=z` for any $x,y,z \in \Sigma^F_0$ (transitivity of popularity)

---

## 7. FOL Theories vs SMT Theories

> ⚠️ **Exam Tip:** The naming difference between FOL theories and SMT theories is a classic exam trap!

| | FOL Theory | SMT Theory |
|---|---|---|
| **Definition** | A set of formulas closed under logical deduction | A set of models interpreting a FOL signature |
| **Example** | All formulas derivable from Peano axioms | All integer arithmetic models |
| **Purpose** | Axiomatize what is provable | Define the domain of interpretation |

- In **FOL**: $T$ is closed under deduction: $\Gamma \models \varphi$ and $\Gamma \subseteq T$ implies $\varphi \in T$.
- In **SMT**: $T$ is a set of models; $\varphi$ is $T$-satisfiable if $\exists \mathcal{M} \in T$ such that $\mathcal{M} \models \varphi$.

> 💡 **Additional Context (from assistant):** In FOL, a theory is about *sentences* (what is true). In SMT, a theory is about *models* (what interpretations exist). SMT asks: "Is there some model (interpretation) in my theory that makes this formula true?"

---

## 8. Expansion and the Ground Satisfiability Problem

In SMT, we check $T$-satisfiability of formulas with **quantifier-free variables**. Since variables play the role of "additional constants", they can be seen as **uninterpreted constants** not in $\Sigma^F_0$.

### Expansion

Given $\Sigma$-model $\mathcal{M} = \langle U, (\cdot)^\mathcal{M} \rangle$ and $\Sigma' \supseteq \Sigma$, an **expansion** of $\mathcal{M}$ to $\Sigma'$ is any $\Sigma'$-model $\mathcal{M}' = \langle U, (\cdot)^{\mathcal{M}'} \rangle$ such that $s^{\mathcal{M}'} = s^\mathcal{M}$ for each $s \in \Sigma$.

Instead of a $\Sigma$-theory $T$, we consider an "expanded" theory:
$$T' = \{\mathcal{M}' \mid \mathcal{M}' \text{ is an expansion of a } \Sigma\text{-model } \mathcal{M}\}
$$

The **ground $T$-satisfiability problem**: given $\Sigma$-theory $T$, determine the $T$-satisfiability of **ground formulas** over a $\Sigma$-expansion $T'$.

> ⚠️ **Exam Tip:** Because uninterpreted constants play the role of variables, our formulas are always technically "ground" (no unbound variables). This is the formal justification for why SMT focuses on ground satisfiability.

**Goal of an SMT solver:** Determine the ground $T$-satisfiability of any formula $\varphi \in F_\Sigma$.

---

## 9. Axiomatic Theory Definitions

Theories can be defined **axiomatically**: given a set of formulas $\Lambda \subseteq F_\Sigma$ (axioms), the corresponding theory is:
$$T_\Lambda = \{\mathcal{M} \mid \forall \varphi \in \Lambda : \varphi^\mathcal{M} = \text{true}\}
$$

### Peano Arithmetic Example

Given $\Sigma$ with constant $0$ and unary function $S$ (successor):
1. $(\forall x)\ \neg(S(x) = 0)$ — 0 has no predecessor
2. $(\forall x)(\forall y)\ S(x) = S(y) \rightarrow x = y$ — $S$ is injective
3. $(\varphi(0) \wedge (\forall x)(\varphi(x) \rightarrow \varphi(S(x)))) \rightarrow (\forall x)\ \varphi(x)$ — Induction schema

This defines **Peano Arithmetic (PA)**. Extending with $+$ and $\ast$ allows proving many arithmetic theorems. However, PA is **incomplete** (Gödel's incompleteness theorems): there exist true arithmetic statements that cannot be proven within PA.

### Many-sorted Logic

Most SMT applications use **many-sorted logic** with:
- A set of **sort symbols** $\mathcal{S}$ (representing different domains/types)
- Sorted variables uniquely associated with a sort $\sigma \in \mathcal{S}$

> 💡 **Additional Context (from assistant):** Think of sorts as **types** in a programming language. Just like Java has `int`, `String`, `float`, many-sorted FOL has sort $\mathbb{Z}$, sort $\text{String}$, sort $\mathbb{R}$. This allows mixing theories cleanly (e.g., string length is an integer, but the string itself is of sort String).

---

## 10. Theories of Interest

### 10.1 EUF — Equality with Uninterpreted Functions

**EUF** ($T_{EUF}$) is also called the **empty theory** (axiom set is $\emptyset$). There are no restrictions on symbol interpretation, except:

**Congruence closure axiom:** For each $\mathbf{x} = (x_1,\ldots,x_k)$, $\mathbf{y} = (y_1,\ldots,y_k)$, and $f \in \Sigma^F_k$:
$$\mathbf{x} = \mathbf{y} \Rightarrow f(\mathbf{x}) = f(\mathbf{y})
$$

**Why EUF?** To abstract complex or "black-box" functions without committing to their specific semantics.

**Example:** Is $a \ast (f(b) + f(c)) = d \wedge b \ast (f(a) + f(c)) \neq d \wedge a = b$ satisfiable?

Abstract $+$ and $\ast$ with fresh uninterpreted functions $g$ and $h$:
$$h(a, g(f(b), f(c))) = d \wedge h(b, g(f(a), f(c))) \neq d \wedge a = b$$

Since $a = b$, by congruence closure: $f(a) = f(b)$, so $g(f(a), f(c)) = g(f(b), f(c))$, so $h(a, g(f(a),f(c))) = h(b, g(f(b),f(c))) = d$ — **contradiction** with the $\neq d$ part. **Unsatisfiable!**

> ⚠️ **Exam Tip:** EUF is used whenever we need to reason about equality and function application without any additional theory — pure structural reasoning.

### 10.2 Linear Arithmetic

Consider signature $\Sigma = (0, 1, +, <)$:

- **$T_{LRA}$** — Quantifier-free theory of **Linear Real Arithmetic**
  - Satisfiability decidable in **polynomial time** (simplex algorithm)
- **$T_{LIA}$** — Quantifier-free theory of **Linear Integer Arithmetic**
  - Satisfiability is **NP-complete** in general

Both $T_{LIA}$ and $T_{LRA}$ are **decidable**.

**Useful fragments with more efficient decision procedures:**

| Fragment | Atom form | Complexity |
|---|---|---|
| **Difference Logic** | $x - y\ \triangleright\!\!\!\triangleleft\ k$ with $\triangleright\!\!\!\triangleleft \in \{=, \leq\}$ | Polynomial (shortest paths) |
| **UTVPI** (Unit Two Variable Per Inequality) | $x \pm y\ \triangleright\!\!\!\triangleleft\ k$ | Polynomial |

> 💡 **Additional Context (from assistant):** Difference logic is like asking "is $x$ at most $k$ more than $y$?" — it can be solved as a shortest-path problem in a constraint graph. UTVPI allows both $x + y$ and $x - y$ forms, which is slightly more expressive.

### 10.3 Nonlinear Arithmetic and Floating Points

With multiplication in $\Sigma = (0, 1, +, \ast, <)$:
- **$T_{NRA}$** (Nonlinear Real Arithmetic): decidable but **doubly exponential** in the worst case (Tarski–Seidenberg)
- **$T_{NIA}$** (Nonlinear Integer Arithmetic): **undecidable** (equivalent to Peano arithmetic)

**Floating-point arithmetic** is tricky: standard algebraic properties break down!

For example, associativity $( x + y) + z = x + (y + z)$ is **not valid** in IEEE 754 floating-point. With $x^\mathcal{M} = 1$, $y^\mathcal{M} = 10^{100}$, $z^\mathcal{M} = -10^{100}$:
- $((x + y) + z)^\mathcal{M} = (10^{100} + (-10^{100})) + 1 = 0 + 1$... wait, actually:
  - $(x + y) = 1 + 10^{100} \approx 10^{100}$ (1 is lost due to limited precision)
  - $(10^{100}) + (-10^{100}) = 0$
  - Result: **0**
- $(x + (y + z))^\mathcal{M} = 1 + (10^{100} - 10^{100}) = 1 + 0 = 1$
- **Catastrophic cancellation:** $0 \neq 1$

![Catastrophic Cancellation](images/slide_30_figure_01_catastrophic_cancellation.png)
> 📌 **Note:** This image is located on **Slide 30** of the provided slides (figure title: "Catastrophic Cancellation"). Please place the file as `images/slide_30_figure_01_catastrophic_cancellation.png` in the same GitHub repository.

> ⚠️ **Exam Tip:** Know why floating-point arithmetic breaks associativity. It's a fundamental difference from mathematical reals and motivates the theory of bit-vectors for hardware-level reasoning.

### 10.4 Theory of Bit-Vectors

**$T_{BV}$** (Theory of Bit-Vectors) handles verification of floating-point computations more reliably than $T_{LRA}$ or $T_{NRA}$.

- **Constants:** Fixed-length arrays of bits (naturally represent machine memory)
- **Operations:**
  - String-like: selection, slicing, concatenation
  - Logical: bitwise NOT, OR, AND, XOR
  - Arithmetic: $+, -, \cdot$
- **Decidability:** Straightforward reduction to SAT via **bit-blasting** (replace each bit-operation with a Boolean formula)

#### Wraparound Example

With `x = 200`, `y = x + 100` as **unsigned 8-bit integers** ($[0, 255]$):
- $y = 300 > 255$: overflow!
- Actual value: $y = 300 \mod 2^8 = 300 \mod 256 = 44$
- So $y < x$ (44 < 200) — the formula $x < y$ is **false** on hardware!

For **signed 16-bit integers** ($[-32768, 32767]$): $32767 + 3 = -32766$ (overflow wraps to negative).

#### Bit-Vector Exercise (from slides)

**Problem:** Are $a, b, c$ bit-vectors with the following TBV-satisfiable?
$$a[0:1] \neq b[0:1] \wedge (a\ |\ b) = c \wedge c[0] = 0 \wedge a[1] + b[1] = 0
$$

**Solution:**
- $c[0] = 0$ means $(a\ |\ b)[0] = a[0] \vee b[0] = 0$, so **both $a[0] = 0$ and $b[0] = 0$**.
- $a[1] + b[1] = 0$ means both bits are 0 (in bit arithmetic, $0+0=0$).
- So $a[0:1] = 00$ and $b[0:1] = 00$, meaning $a[0:1] = b[0:1]$.
- This **contradicts** $a[0:1] \neq b[0:1]$. **Unsatisfiable!**

> 💡 **Additional Context (from assistant):** Bit-blasting means turning each bit-vector formula into a purely Boolean SAT problem. For a 32-bit formula, each variable becomes 32 Boolean variables. This is why very large bit-widths can make SMT slow — the SAT instance grows proportionally.

### 10.5 Theory of Arrays

$T_{BV} \neq T_A$ — these are different theories!

**$T_A$** (Theory of Arrays) treats arrays as **dictionaries** mapping keys of sort $K$ to values of sort $V$. Keys are not necessarily non-negative integers.

**Main operations:**
- `read(a, i)` — returns value of $a[i]$
- `write(a, i, v)` — returns array with $a[i]$ replaced by $v$

**Axioms of $T_A$ (with extensionality):**

1. $(\forall a)(\forall i)(\forall v)\ \text{read}(\text{write}(a, i, v), i) = v$
2. $(\forall a)(\forall i)(\forall j)(\forall v)\ i \neq j \rightarrow \text{read}(\text{write}(a, i, v), j) = \text{read}(a, j)$
3. $(\forall a)(\forall a')\ ((\forall i)\ \text{read}(a, i) = \text{read}(a', i)) \rightarrow a = a'$ (extensionality)

> ⚠️ **Exam Tip:** These three axioms are frequently tested. Axiom (i) is "reading back what you wrote", axiom (ii) is "writing doesn't affect other indices", and axiom (iii) is "two arrays equal iff they agree pointwise".

#### Array Exercises (from slides — all solved)

**Exercise 1:** Is $\text{write}(a, i, x) \neq b \wedge a = b \wedge i = j \wedge \text{read}(b, i) = y \wedge \text{read}(\text{write}(b, i, x), j) = y$ satisfiable?

**Solution:**
- From $a = b$ and $i = j$: substituting, we need $\text{write}(a, i, x) \neq a$ and $\text{read}(\text{write}(a, i, x), i) = y$ and $\text{read}(a, i) = y$.
- By axiom (i): $\text{read}(\text{write}(a, i, x), i) = x$, so $x = y$.
- $\text{read}(a, i) = y = x$.
- Now apply the last solved exercise below... actually this leads us to the next exercise. **See Exercise 4 for the core proof.**

**Exercise 2:** Is $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = y \wedge \text{read}(\text{write}(a, i, x), i) = y$ satisfiable?

**Solution:**
- By axiom (i): $\text{read}(\text{write}(a, i, x), i) = x$, so $x = y$.
- We also have $\text{read}(a, i) = y = x$.
- So this reduces to: $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = x$. **See Exercise 4.**

**Exercise 3:** Is $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = y \wedge x = y$ satisfiable?

**Solution:**
- From $x = y$ and $\text{read}(a, i) = y$: we get $\text{read}(a, i) = x$.
- Reduces to: $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = x$. **See Exercise 4.**

**Exercise 4 (core):** Is $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = x$ satisfiable?

**Solution (full derivation from slides):**

Let $a' = \text{write}(a, i, x)$. Then $a' \neq a$.

By axiom (iii) (contrapositive): since $a' \neq a$, there must exist $j$ such that $\text{read}(a', j) \neq \text{read}(a, j)$.

**Case 1: $i \neq j$**
- By axiom (ii): $\text{read}(a', j) = \text{read}(\text{write}(a,i,x), j) = \text{read}(a, j)$
- This **contradicts** $\text{read}(a', j) \neq \text{read}(a, j)$. ✗

**Case 2: $i = j$**
- $\text{read}(a', i) \neq \text{read}(a, i)$
- By axiom (i): $\text{read}(a', i) = \text{read}(\text{write}(a,i,x), i) = x$
- So $x \neq \text{read}(a, i)$
- But we have $\text{read}(a, i) = x$ — **contradiction!** ✗

Both cases lead to contradiction. **Formula is UNSATISFIABLE.**

> 💡 **Additional Context (from assistant):** The undecidability of the full $T_A$ comes from universal quantifiers in the extensionality axiom. In practice, SMT solvers handle decidable fragments of $T_A$ using the **array property fragment** or by eliminating quantifiers eagerly. Arrays are extremely useful for **memory abstraction** in software verification.

### 10.6 Theory of Strings

**Word equations** are fundamental to string solving. Fixed an alphabet $\mathcal{S}$, a word equation has the form $L = R$ where $L, R$ are concatenations of string variables and constants from $\mathcal{S}^*$.

Example: $X \cdot \text{world} \cdot Z = \text{hello} \cdot Y$

- Full FO theory of word equations: **undecidable** (equivalent to arithmetic)
- Quantifier-free theory of word equations: **decidable**

#### String Exercises (from slides — solved)

**Exercise 1:** Is $XY = YX \wedge X \neq Y$ satisfiable?

**Solution:** Yes, **satisfiable**. Set $X = \epsilon$ (empty string), $Y = a$ (any string).
- $\epsilon \cdot a = a = a \cdot \epsilon$ ✓
- $\epsilon \neq a$ ✓

**Exercise 2:** Is $aX = Xb \wedge a \neq b$ satisfiable?

**Solution:** **Unsatisfiable.** Proof by infinite descent:
- $aX = Xb$ means $X$ starts with $a$ and ends with $b$, so $X = aX_1b$.
- Then $a(aX_1b) = (aX_1b)b$, i.e., $aaX_1b = aX_1bb$, so $aX_1 = X_1b$ with $|X_1| < |X|$.
- By induction: $X_k = a^k X_0 b^k$, and $|X_k|$ decreases but must always contain both $a$ as prefix and $b$ as suffix — impossible when $|X_k| < |a| + |b| = 2$. **Contradiction!**

Strings can expose **security vulnerabilities** in web programs (e.g., SQL injection, XSS). Modern string theories handle complex operations on **unbounded-length strings**, often combined with:
- Arithmetic theory (for string length)
- Regular expressions

> 💡 **Additional Context (from assistant):** The string theory is used extensively in **web security analysis**. For example, an SMT solver can check whether a user input could bypass a filter by modeling the string manipulation as constraints and asking: is there a string $X$ that satisfies all the filter conditions AND is a SQL injection payload?

---

## 11. SMT-LIB Encoding

**SMT-LIB** is the standard language for encoding SMT problems, used by Z3, CVC5, and others.

Example (theory of arrays, satisfiability check):
```smt2
; Signature expansion
(declare-fun a () (Array Int Real))
(declare-fun b () (Array Int Real))
(declare-fun i () Int)
(declare-fun j () Int)
(declare-fun x () Real)
(declare-fun y () Real)
; Formulas. select = read, store = write
(assert (not (= (store a i x) b)))
(assert (= (select b i) y))
(assert (= (select (store b i x) j) y))
(assert (= a b))
(assert (= i j))
; Checking satisfiability
(check-sat)
```

Key SMT-LIB mappings:
- `select` ↔ `read`
- `store` ↔ `write`
- `declare-fun` declares a new uninterpreted symbol (function or constant)
- `assert` adds a formula as a constraint
- `check-sat` runs the solver

---

## 12. Take-Home Messages

1. **SMT extends SAT** to solve formulas in quantifier-free FOL: functions, constants, predicates with arity > 1.
2. SMT is **similar to / orthogonal from CP**: tackles combinatorial problems from a "more logical" perspective (formulas vs. constraints).
3. SMT is more **oriented toward software analysis** and less toward optimization.
4. Eventually, SMT solving (eagerly or lazily) **relies on SAT solving**:
   > SAT : machine language ≈ SMT : higher-level language
5. Several theories of interest: **EUF, arithmetic, arrays, bit-vectors, strings**, ...

---

## 📝 Review Questions (30 Questions)

### Foundational Concepts

**Q1. ⚠️ What is SMT and how does it extend SAT?**

**Answer:** SMT (Satisfiability Modulo Theories) studies the satisfiability of quantifier-free FOL formulas with respect to fixed background theories (e.g., arithmetic, arrays, strings). SAT only handles propositional (Boolean) logic. SMT extends SAT by adding expressive domain-specific reasoning while still leveraging SAT solvers under the hood.

---

**Q2. Why is propositional logic insufficient for many real-world applications?**

**Answer:** Propositional logic only models Boolean atoms (true/false facts). It cannot express numeric relationships like $x + 1 = y$, array access like $a[i] = v$, or string patterns. These require First-Order Logic over appropriate background theories.

---

**Q3. ⚠️ What is the difference between SAT, CP, and SMT?**

**Answer:**
- **SAT**: Propositional logic, NP-complete, Boolean domains only, no domain reasoning.
- **CP**: Constraint programming, domain-specific propagators, finite/bounded domains, good for scheduling/resource allocation.
- **SMT**: FOL-based, combines domain-specific reasoning from multiple theories, handles infinite domains, best for program analysis/verification.

---

**Q4. What does it mean for a formula to be T-satisfiable?**

**Answer:** A formula $\varphi$ is $T$-satisfiable if there exists a model $\mathcal{M} \in T$ such that $\varphi^\mathcal{M} = \text{true}$. That is, there is an interpretation in the theory that makes the formula true.

---

**Q5. Define: atom, literal, clause, CNF.**

**Answer:**
- **Atom:** An atomic formula (e.g., $p(t_1,\ldots,t_k)$, $t_1 = t_2$, $\bot$, $\top$)
- **Literal:** An atom (positive literal) or its negation (negative literal)
- **Clause:** A disjunction of literals: $\ell_1 \vee \cdots \vee \ell_k$
- **CNF:** A conjunction of clauses: $C_1 \wedge \cdots \wedge C_k$

---

**Q6. What is a FOL signature? Give an example.**

**Answer:** A signature $\Sigma = \Sigma^F \cup \Sigma^P$ is the set of non-logical symbols: $\Sigma^F$ contains function symbols (including constants as 0-ary functions), $\Sigma^P$ contains predicate symbols. Example: $\Sigma^F = \{0, 1, +\}$, $\Sigma^P = \{<, =\}$ for linear arithmetic.

---

**Q7. ⚠️ What is the difference between an FOL theory and an SMT theory?**

**Answer:**
- **FOL theory:** A set of *formulas* closed under logical deduction (e.g., all consequences of Peano axioms).
- **SMT theory:** A set of *models* interpreting a FOL signature (e.g., all integer arithmetic models).

SMT asks: is there a *model* in $T$ satisfying $\varphi$? FOL asks: is $\varphi$ derivable from the theory's axioms?

---

**Q8. What is an expansion of a model? Why is it useful in SMT?**

**Answer:** Given $\Sigma$-model $\mathcal{M}$ and $\Sigma' \supseteq \Sigma$, an expansion of $\mathcal{M}$ to $\Sigma'$ is a $\Sigma'$-model that agrees with $\mathcal{M}$ on all $\Sigma$ symbols but freely interprets the new $\Sigma' \setminus \Sigma$ symbols. This allows variables (as uninterpreted constants) to be freely assigned values, making SMT formulas effectively "ground".

---

### Dynamic Symbolic Execution

**Q9. ⚠️ What is Dynamic Symbolic Execution (DSE)? What is it used for?**

**Answer:** DSE (also called concolic execution) runs a program with concrete inputs while simultaneously tracking symbolic expressions. It builds **path conditions (PC)** — constraints on inputs that lead to a specific execution path. By negating PC constraints and solving them with an SMT solver, DSE generates new test inputs that cover different execution paths. It is used for **automated test generation** and **bug detection**.

---

**Q10. In the DSE JavaScript example, how are all three execution paths covered?**

**Answer:**
1. Start with $x = ""$: reaches PC3, PC = $\{\neg(|x| \geq 2), \neg(\text{"length"}[x] = \text{"g"})\}$
2. Negate $\neg(|x| \geq 2)$: solve $|x| \geq 2$, get $x = \text{"aa"}$, reaches PC1
3. Negate $\neg(\text{"length"}[x] = \text{"g"})$: solve $\text{"length"}[x] = \text{"g"}$, get $x = \text{"3"}$ (since `"length"[3] === "g"`), reaches PC2

All three paths covered by $\{\text{""}, \text{"aa"}, \text{"3"}\}$.

---

### EUF Theory

**Q11. ⚠️ What is the EUF theory? What is the congruence closure property?**

**Answer:** EUF (Equality with Uninterpreted Functions) has no axioms other than congruence closure: if $\mathbf{x} = \mathbf{y}$ then $f(\mathbf{x}) = f(\mathbf{y})$ for any function $f$. It allows reasoning about equality and function application without knowing the functions' definitions. Useful for abstracting complex or black-box functions.

---

**Q12. Is $f(a) = b \wedge f(b) = a \wedge a \neq b$ EUF-satisfiable? Explain.**

**Answer:** Yes, **satisfiable**. There is no contradiction: $a$ and $b$ are distinct, $f$ maps $a \to b$ and $b \to a$ (a bijection/swap). EUF only requires congruence: since $a = a$, we need $f(a) = f(a) = b$, which is consistent. No axiom forces $a = b$.

---

**Q13. Show that $g(x, y) = z \wedge g(y, x) \neq z \wedge x = y$ is EUF-unsatisfiable.**

**Answer:**
- From $x = y$: by congruence closure, $g(x, y) = g(y, x)$.
- We have $g(x, y) = z$, so $g(y, x) = z$.
- But we also have $g(y, x) \neq z$. **Contradiction!** ✗

---

### Linear Arithmetic

**Q14. ⚠️ What is the difference between $T_{LRA}$ and $T_{LIA}$? Which is harder?**

**Answer:**
- **$T_{LRA}$**: Linear Real Arithmetic — satisfiability decidable in **polynomial time** (Simplex).
- **$T_{LIA}$**: Linear Integer Arithmetic — satisfiability is **NP-complete**.

$T_{LIA}$ is harder because integers require checking that a feasible real solution can be "rounded" to an integer, which is the essence of integer programming.

---

**Q15. What is difference logic? Give an example formula.**

**Answer:** In difference logic, every atom has the form $x - y\ \triangleright\!\!\!\triangleleft\ k$ where $\triangleright\!\!\!\triangleleft \in \{=, \leq\}$, $x, y$ are variables, and $k$ is an integer constant. Example: $x - y \leq 3 \wedge y - z \leq 2 \wedge z - x \leq -4$. Decidable in polynomial time via shortest-path algorithms on constraint graphs.

---

**Q16. Is $x - y \leq 5 \wedge y - z \leq 3 \wedge z - x \leq -9$ satisfiable in difference logic?**

**Answer:** Sum the inequalities: $(x-y) + (y-z) + (z-x) \leq 5 + 3 + (-9) = -1$. But the left-hand side telescopes to $0$, so we get $0 \leq -1$. **Contradiction! Unsatisfiable.**

---

### Bit-Vectors

**Q17. ⚠️ Why does associativity of addition fail for IEEE 754 floating-point numbers?**

**Answer:** Floating-point numbers have **limited precision** (finite mantissa bits). When adding a very small number to a very large number, the small number may be "lost" (not representable in the result's precision). This is **catastrophic cancellation**. For example, $(1 + 10^{100}) + (-10^{100}) = 0$ in IEEE 754, while $1 + (10^{100} - 10^{100}) = 1$.

---

**Q18. What is bit-blasting in the theory of bit-vectors?**

**Answer:** Bit-blasting is the reduction of a bit-vector formula to an equivalent Boolean SAT formula by replacing each bit of each bit-vector with an individual Boolean variable and each bit-operation with its Boolean circuit equivalent. This allows any SAT solver to handle bit-vector problems, but the size of the resulting formula grows proportionally with bit-width.

---

**Q19. With 8-bit unsigned arithmetic, compute $200 + 100 \mod 2^8$.**

**Answer:** $200 + 100 = 300$. Since $300 > 255 = 2^8 - 1$, overflow occurs. $300 \mod 256 = 44$. So $y = 44 < 200 = x$, and the formula $y > x$ is **false** on 8-bit hardware despite being true in standard arithmetic.

---

### Theory of Arrays

**Q20. ⚠️ State the three axioms of the Theory of Arrays $T_A$.**

**Answer:**
1. $\forall a, i, v:\ \text{read}(\text{write}(a, i, v), i) = v$ (read-over-same-write)
2. $\forall a, i, j, v:\ i \neq j \rightarrow \text{read}(\text{write}(a, i, v), j) = \text{read}(a, j)$ (write doesn't affect other indices)
3. $\forall a, a':\ (\forall i:\ \text{read}(a, i) = \text{read}(a', i)) \rightarrow a = a'$ (extensionality)

---

**Q21. ⚠️ Prove that $\text{write}(a, i, x) \neq a \wedge \text{read}(a, i) = x$ is $T_A$-unsatisfiable.**

**Answer:** Let $a' = \text{write}(a, i, x)$. Since $a' \neq a$, by axiom (iii) (contrapositive), $\exists j: \text{read}(a', j) \neq \text{read}(a, j)$.
- **Case $i \neq j$:** Axiom (ii) gives $\text{read}(a', j) = \text{read}(a, j)$ — contradiction.
- **Case $i = j$:** Axiom (i) gives $\text{read}(a', i) = x$. So $x \neq \text{read}(a, i)$, contradicting $\text{read}(a, i) = x$.

Both cases yield contradictions, so **unsatisfiable**.

---

**Q22. Why is the full Theory of Arrays undecidable?**

**Answer:** The undecidability comes from the **extensionality axiom (iii)**, which contains a universal quantifier $\forall i$. This makes the full $T_A$ undecidable. In practice, SMT solvers handle decidable fragments (e.g., quantifier-free array theory by eager quantifier elimination or the array property fragment).

---

**Q23. What is the main advantage of $T_A$ over $T_{BV}$ for memory abstraction?**

**Answer:** In $T_A$, the array abstraction depends on the **number of accesses** to memory rather than its actual size. So you can reason about an unbounded or very large memory without modeling every cell. $T_{BV}$ imposes a **fixed length** on its arrays, making it unsuitable for unbounded memory.

---

### Theory of Strings

**Q24. ⚠️ Is $XY = YX \wedge X \neq Y$ satisfiable in the quantifier-free theory of word equations?**

**Answer:** Yes, **satisfiable**. Set $X = \epsilon$ (empty string), $Y = a$ (any non-empty string): $\epsilon \cdot a = a = a \cdot \epsilon$, and $\epsilon \neq a$.

---

**Q25. Prove that $aX = Xb \wedge a \neq b$ is unsatisfiable.**

**Answer:** If $aX = Xb$, then $X$ must start with $a$ (from the left side) and end with $b$ (from the right side). Write $X = aX_1b$. Substituting: $a(aX_1b) = (aX_1b)b$ → $aX_1 = X_1b$ with $|X_1| = |X| - 2$. By induction, $|X_k| = |X| - 2k$. Eventually $|X_k| < 2$, but $X_k$ must start with $a$ and end with $b$ — impossible for length $< 2$ when $a \neq b$. **Unsatisfiable.**

---

**Q26. What are the practical applications of string theories in SMT?**

**Answer:** String theories are used in: (1) **web security analysis** — modeling string manipulations to detect SQL injection or XSS vulnerabilities; (2) **dynamic symbolic execution** — generating test inputs for string-processing code; (3) **program verification** — verifying string-based protocols. Before dedicated string theories, strings were modeled with automata (limited expressiveness) or bit-vectors (fixed length bounds).

---

### Advanced / Mixed

**Q27. ⚠️ What is the ground T-satisfiability problem in SMT?**

**Answer:** Given a $\Sigma$-theory $T$, the ground $T$-satisfiability problem is: does a given ground formula (a formula where all symbols are constants — including uninterpreted variables treated as constants) have a model in the expansion $T'$ of $T$? Since variables are treated as uninterpreted constants, all SMT formulas are effectively "ground."

---

**Q28. Explain why many-sorted logic is important for SMT.**

**Answer:** Real-world SMT problems involve multiple data types (integers, strings, arrays, booleans). Many-sorted logic associates each variable with a **sort** (type), enabling clean mixing of theories. For example, a formula can simultaneously use integer arithmetic for array indices and string theory for array values, with the sort system preventing type mismatches.

---

**Q29. What is UTVPI and why might it be preferred over full $T_{LIA}$?**

**Answer:** UTVPI (Unit Two Variable Per Inequality) is a fragment of $T_{LIA}$ where every atom has the form $x \pm y\ \triangleright\!\!\!\triangleleft\ k$. It is decidable in **polynomial time** (unlike full $T_{LIA}$ which is NP-complete), making it much more efficient for problems whose constraints fit this restricted form. It is commonly used in difference logic extensions and program analysis.

---

**Q30. ⚠️ Summarize the decidability status of key SMT theories.**

**Answer:**

| Theory | Decidability | Complexity |
|---|---|---|
| EUF | Decidable | Polynomial |
| $T_{LRA}$ (Linear Real Arithmetic) | Decidable | Polynomial |
| $T_{LIA}$ (Linear Integer Arithmetic) | Decidable | NP-complete |
| Difference Logic | Decidable | Polynomial |
| UTVPI | Decidable | Polynomial |
| $T_{NRA}$ (Nonlinear Real Arithmetic) | Decidable | 2-EXPTIME |
| $T_{NIA}$ (Nonlinear Integer Arithmetic) | **Undecidable** | — |
| $T_{BV}$ (Bit-vectors) | Decidable (via SAT) | NP-complete |
| $T_A$ (Arrays, quantifier-free) | Decidable | NP-complete |
| $T_A$ (Arrays, full) | **Undecidable** | — |
| Word equations (QF) | Decidable | PSPACE |
| Word equations (full FO) | **Undecidable** | — |

---

*Notes compiled by AI Study Assistant for Kaveh HajiNajaf | University of Bologna | Module 2: SMT Introduction*
