# Linear Algebra Prerequisites for AI — Study Notes
### Course: Statistical and Mathematical Methods for AI | University of Bologna
### Instructor: Elena Loli Piccolomini | A.A. 2020/2021

---

## Table of Contents
1. [Vector Spaces](#1-vector-spaces)
2. [Subspaces](#2-subspaces)
3. [Linear Independence and Span](#3-linear-independence-and-span)
4. [Basis and Dimension](#4-basis-and-dimension)
5. [Matrices — Definitions](#5-matrices--definitions)
6. [Matrix Operations](#6-matrix-operations)
7. [Special Matrices](#7-special-matrices)
8. [Determinant of a Matrix](#8-determinant-of-a-matrix)
9. [Matrix Inverse via Cofactors](#9-matrix-inverse-via-cofactors)
10. [Conditions for Invertibility](#10-conditions-for-invertibility)
11. [Eigenvalues and Eigenvectors](#11-eigenvalues-and-eigenvectors)
12. [Spectral Radius](#12-spectral-radius)
13. [Norms on Vector Spaces](#13-norms-on-vector-spaces)
14. [Matrix Norms](#14-matrix-norms)
15. [📝 Review Questions (30 Questions)](#-review-questions-30-questions)

---

## 1. Vector Spaces

A **vector space** over a field $\mathbb{F}$ ($\mathbb{F} = \mathbb{R}$ or $\mathbb{F} = \mathbb{C}$) is a set $V$ closed under vector addition and scalar multiplication, satisfying 8 axioms:

| Axiom | Statement |
|---|---|
| Commutativity of addition | $\forall v, w \in V: v + w = w + v$ |
| Associativity of addition | $\forall u, v, w \in V: u + (v+w) = (u+v) + w$ |
| Additive identity | $\exists\, \mathbf{0} \in V: v + \mathbf{0} = v$ |
| Additive inverse | $\forall v \in V, \exists\, (-v) \in V: v + (-v) = \mathbf{0}$ |
| Multiplicative identity | $\exists\, 1 \in \mathbb{F}: 1 \cdot v = v$ |
| Compatibility of scalar multiplication | $(ab)v = a(bv)$ |
| Distributivity over field addition | $(a+b)v = av + bv$ |
| Distributivity over vector addition | $a(v+w) = av + aw$ |

> ⚠️ **Exam Tip:** You may be asked to **verify** whether a given set with operations is a vector space by checking these 8 axioms. Know them precisely.

### Notable Examples of Vector Spaces

- $\mathbb{R}^n$: $n$-tuples of real numbers
- $\mathbb{C}^n$: $n$-tuples of complex numbers
- $\mathcal{P}_n$: polynomials of degree $\leq n$
- $\mathcal{C}^n([a,b])$: real/complex-valued functions continuous on $[a,b]$ up to their $n$-th derivative

> 💡 **Additional Context (from assistant):** In AI/ML, the most important example is $\mathbb{R}^n$ — your feature vectors, weight vectors, and gradient vectors all live in $\mathbb{R}^n$. Understanding vector spaces underpins everything from linear regression to neural networks.

---

## 2. Subspaces

**Definition:** $W$ is a **subspace** of $V$ if and only if:
- $W \subset V$
- $W$ is itself a vector space over $\mathbb{F}$

In practice, $W$ is a subspace iff it is **closed under addition and scalar multiplication**:
- $\forall w_1, w_2 \in W: w_1 + w_2 \in W$
- $\forall \alpha \in \mathbb{F}, w \in W: \alpha w \in W$

### Example — Is it a subspace?

**Yes:** $V = \mathbb{R}^3$, $W = \{(\alpha, 0, 0) : \alpha \in \mathbb{R}\}$
- $(\alpha,0,0) + (\beta,0,0) = (\alpha+\beta,0,0) \in W$ ✓
- $c(\alpha,0,0) = (c\alpha,0,0) \in W$ ✓

**No:** $V = \mathbb{R}^3$, $U = \{(1,0,0), (1,2,3)\}$
- $(1,0,0) + (1,2,3) = (2,2,3) \notin U$ ✗ (not closed under addition)

> 💡 **Additional Context (from assistant):** The null space (kernel) and column space (range) of a matrix are both subspaces — this becomes essential when studying linear systems $Ax = b$ and their solvability.

---

## 3. Linear Independence and Span

### Span

The **span** of a set $\{v_1, \ldots, v_m\} \subset V$ is:
$$W = \text{span}\{v_1, \ldots, v_m\} = \left\{\sum_{i=1}^{m} \alpha_i v_i \;\middle|\; v_i \in V,\, \alpha_i \in \mathbb{F}\right\}$$

The system $\{v_1, \ldots, v_m\}$ is called a **system of generators** for $W$.

**Example:** $\{(-1,0,0), (0,1,0), (0,0,2), (-1,0,4)\}$ is a system of generators for $\mathbb{R}^3$.

### Linear Independence

> ⚠️ **Exam Tip:** Linear independence is tested on nearly every exam. Know the definition and how to apply it.

The system $\{v_1, \ldots, v_m\}$ is **linearly independent** if:
$$\alpha_1 v_1 + \cdots + \alpha_m v_m = \mathbf{0} \implies \alpha_1 = \alpha_2 = \cdots = \alpha_m = 0$$

Otherwise, the system is **linearly dependent** (at least one vector can be written as a linear combination of the others).

**Geometric intuition:** $n$ vectors are linearly dependent if they all lie on the same $(n-1)$-dimensional hyperplane.

### Worked Example

$V = \mathbb{R}^2$, $v_1 = (1,2)$, $v_2 = (3,4)$. Are they linearly independent?

$$\alpha_1(1,2) + \alpha_2(3,4) = (0,0) \implies \begin{cases} \alpha_1 + 3\alpha_2 = 0 \\ 2\alpha_1 + 4\alpha_2 = 0 \end{cases}$$

Solving: from first equation $\alpha_1 = -3\alpha_2$; substituting: $-6\alpha_2 + 4\alpha_2 = 0 \implies \alpha_2 = 0 \implies \alpha_1 = 0$.

Since the only solution is $\alpha_1 = \alpha_2 = 0$, the vectors are **linearly independent**. ✓

### 3 Additional Exercises

**Exercise A.** Are $v_1 = (1,0,0)$, $v_2 = (0,1,0)$, $v_3 = (1,1,0)$ in $\mathbb{R}^3$ linearly independent?

$$\alpha_1(1,0,0) + \alpha_2(0,1,0) + \alpha_3(1,1,0) = (0,0,0)$$
$$\Rightarrow \begin{cases} \alpha_1 + \alpha_3 = 0 \\ \alpha_2 + \alpha_3 = 0 \\ 0 = 0 \end{cases}$$

This system has infinitely many solutions (e.g., $\alpha_3 = 1, \alpha_1 = -1, \alpha_2 = -1$). **Linearly dependent.** ($v_3 = v_1 + v_2$)

**Exercise B.** Are $v_1 = (1,2,3)$, $v_2 = (4,5,6)$, $v_3 = (7,8,9)$ linearly independent?

The determinant of the $3\times3$ matrix with these rows:
$$\det = 1(5\cdot9 - 6\cdot8) - 2(4\cdot9 - 6\cdot7) + 3(4\cdot8 - 5\cdot7) = 1(-3) - 2(-6) + 3(-3) = -3 + 12 - 9 = 0$$

$\det = 0$ implies **linearly dependent**.

**Exercise C.** Are $v_1 = (1,0)$, $v_2 = (0,1)$, $v_3 = (2,3)$ in $\mathbb{R}^2$ a linearly independent set?

There are 3 vectors in a 2-dimensional space. By the dimension theorem, any set of more than $n = \dim(V)$ vectors must be **linearly dependent**. ✗

---

## 4. Basis and Dimension

**Definition:** A **basis** of $V$ is any system of vectors that is:
1. **Linearly independent**, and
2. **Spans** $V$ (i.e., is a system of generators for $V$)

**Proposition:** If $V$ admits a basis of $n$ vectors, then:
- Every linearly independent system has **at most** $n$ elements
- Every other basis also has **exactly** $n$ elements
- The number $n$ is the **dimension** of $V$, written $\dim(V) = n$

**Example:** $\{(1,0,0), (0,1,0), (0,0,1)\}$ is the **standard basis** for $\mathbb{R}^3$, so $\dim(\mathbb{R}^3) = 3$.

> 💡 **Additional Context (from assistant):** The dimension of a vector space equals the minimum number of coordinates needed to uniquely identify any point in it. In machine learning, feature dimensionality directly corresponds to this concept. A model with 1000 features operates in a 1000-dimensional vector space.

---

## 5. Matrices — Definitions

A **matrix** $A \in \mathbb{F}^{m \times n}$ is a rectangular array of $m$ rows and $n$ columns:
$$A = \begin{pmatrix} a_{11} & a_{12} & \cdots & a_{1n} \\ a_{21} & a_{22} & \cdots & a_{2n} \\ \vdots & & \ddots & \vdots \\ a_{m1} & a_{m2} & \cdots & a_{mn} \end{pmatrix}$$

Key terms:
- **Square matrix:** $m = n$
- **Main diagonal:** entries $a_{ij}$ with $i = j$
- **Rank** $\text{rank}(A)$: maximum number of linearly independent columns (= rows)
- **Full rank:** $\text{rank}(A) = \min(m,n)$

### Triangular Matrices

- **Lower triangular** $L$: $l_{ij} = 0$ if $i < j$ (zeros above diagonal)
- **Upper triangular** $U$: $u_{ij} = 0$ if $i > j$ (zeros below diagonal)

> ⚠️ **Exam Tip:** Triangular matrices have special properties — their determinant is the product of diagonal entries, and they arise naturally in LU decomposition (later chapters).

---

## 6. Matrix Operations

Let $A, B \in \mathbb{R}^{m \times p}$, $C \in \mathbb{R}^{p \times n}$, $\lambda \in \mathbb{F}$:

| Operation | Formula | Result size |
|---|---|---|
| Addition | $(A+B)_{ij} = a_{ij} + b_{ij}$ | $m \times p$ |
| Scalar multiplication | $(\lambda A)_{ij} = \lambda a_{ij}$ | $m \times p$ |
| Matrix multiplication | $(AC)_{ij} = \sum_{k=1}^p a_{ik} c_{kj}$ | $m \times n$ |
| Transposition | $(A^T)_{ij} = a_{ji}$ | $p \times m$ |

**Matrix multiplication** requires the inner dimensions to match: $(m \times p) \cdot (p \times n) = (m \times n)$.

**Important:** Matrix product is **NOT commutative** in general: $AC \neq CA$.

**Transposition properties:**
$$(A^T)^T = A, \quad (A+B)^T = A^T + B^T, \quad (AC)^T = C^T A^T, \quad (\lambda A)^T = \lambda A^T$$

### Worked Example

$$A = \begin{pmatrix}1 & 2 & 3 \\ 4 & 5 & 6\end{pmatrix}, \quad B = \begin{pmatrix}7 & 8 & 9 \\ 1 & 2 & 3\end{pmatrix}$$

$$A + B = \begin{pmatrix}8 & 10 & 12 \\ 5 & 7 & 9\end{pmatrix}$$

$$B^T = \begin{pmatrix}7 & 1 \\ 8 & 2 \\ 9 & 3\end{pmatrix}, \quad AB^T = \begin{pmatrix}1\cdot7+2\cdot8+3\cdot9 & 1\cdot1+2\cdot2+3\cdot3 \\ 4\cdot7+5\cdot8+6\cdot9 & 4\cdot1+5\cdot2+6\cdot3\end{pmatrix} = \begin{pmatrix}50 & 14 \\ 122 & 32\end{pmatrix}$$

### 3 Additional Exercises

**Exercise A.** Compute $AB$ where $A = \begin{pmatrix}1 & 2 \\ 3 & 4\end{pmatrix}$, $B = \begin{pmatrix}0 & 1 \\ 1 & 0\end{pmatrix}$.

$$AB = \begin{pmatrix}0+2 & 1+0 \\ 0+4 & 3+0\end{pmatrix} = \begin{pmatrix}2 & 1 \\ 4 & 3\end{pmatrix}$$

**Exercise B.** Show that $AB \neq BA$ for the matrices above.

$$BA = \begin{pmatrix}0\cdot1+1\cdot3 & 0\cdot2+1\cdot4 \\ 1\cdot1+0\cdot3 & 1\cdot2+0\cdot4\end{pmatrix} = \begin{pmatrix}3 & 4 \\ 1 & 2\end{pmatrix} \neq \begin{pmatrix}2 & 1 \\ 4 & 3\end{pmatrix} = AB$$

**Exercise C.** Verify $(AB)^T = B^T A^T$ for the matrices in Exercise A.

$(AB)^T = \begin{pmatrix}2 & 4 \\ 1 & 3\end{pmatrix}$

$B^T A^T = \begin{pmatrix}0 & 1 \\ 1 & 0\end{pmatrix}\begin{pmatrix}1 & 3 \\ 2 & 4\end{pmatrix} = \begin{pmatrix}2 & 4 \\ 1 & 3\end{pmatrix}$ ✓

---

## 7. Special Matrices

| Name | Definition | Property |
|---|---|---|
| **Diagonal** | $A = \text{diag}(d_{11}, \ldots, d_{nn})$; $a_{ij} = 0$ for $i \neq j$ | Simplifies many computations |
| **Identity** $I_n$ | $I_n = \text{diag}(1,1,\ldots,1)$ | $AI = IA = A$ for all square $A$ |
| **Invertible (nonsingular)** | $\exists B: AB = BA = I$; write $B = A^{-1}$ | Columns are linearly independent |
| **Singular** | Not invertible | $\det(A) = 0$ |
| **Symmetric** | $A = A^T$ | |
| **Antisymmetric** | $A = -A^T$ | Diagonal entries are zero |
| **Orthogonal** | $A^{-1} = A^T$, i.e., $AA^T = A^TA = I$ | $\det(A) = \pm 1$ |

> ⚠️ **Exam Tip:** Know the definitions of symmetric, antisymmetric, and orthogonal matrices and be able to verify whether a given matrix belongs to each class.

**Inverse properties:**
- $(A^{-1})^{-1} = A$
- $(AB)^{-1} = B^{-1}A^{-1}$ (reverse order!)
- $(A^T)^{-1} = (A^{-1})^T = A^{-T}$

---

## 8. Determinant of a Matrix

For $A \in \mathbb{C}^{n \times n}$, the **determinant** $\det(A)$ is defined recursively via **Laplace's rule** (cofactor expansion):

$$\det(A) = \sum_{j=1}^{n} (-1)^{i+j} \det(A_{ij}) \cdot a_{ij}$$

where $A_{ij}$ is the $(n-1)\times(n-1)$ submatrix obtained by deleting row $i$ and column $j$.

**Shortcut for triangular/diagonal matrices:** $\det(A) = \prod_{i=1}^n a_{ii}$

**Key properties:**
$$\det(A) = \det(A^T), \quad \det(AB) = \det(A)\det(B), \quad \det(A^{-1}) = \frac{1}{\det(A)}, \quad \det(\alpha A) = \alpha^n \det(A)$$

> ⚠️ **Exam Tip:** Computing determinants using Sarrus' rule ($3\times3$) and Laplace expansion are standard exam tasks.

### $2 \times 2$ Determinant

$$\det\begin{pmatrix}a & b \\ c & d\end{pmatrix} = ad - bc$$

### $3 \times 3$ Determinant (Sarrus' Rule)

For $A = \begin{pmatrix}a_{11}&a_{12}&a_{13}\\a_{21}&a_{22}&a_{23}\\a_{31}&a_{32}&a_{33}\end{pmatrix}$:

$$\det(A) = a_{11}a_{22}a_{33} + a_{12}a_{23}a_{31} + a_{21}a_{32}a_{13} - a_{13}a_{22}a_{31} - a_{12}a_{21}a_{33} - a_{23}a_{32}a_{11}$$

### Worked Example (from notes)

$$A = \begin{pmatrix}1 & 2 & 0 \\ 3 & 4 & 5 \\ 3 & 0 & 4\end{pmatrix}$$

$$\det(A) = 1\cdot4\cdot4 + 2\cdot5\cdot3 + 3\cdot0\cdot0 - 0\cdot4\cdot3 - 2\cdot3\cdot4 - 5\cdot0\cdot1 = 16 + 30 + 0 - 0 - 24 - 0 = 22$$

### 3 Additional Exercises

**Exercise A.** Compute $\det\begin{pmatrix}3 & 1 \\ 2 & 5\end{pmatrix}$.
$$\det = 3 \cdot 5 - 2 \cdot 1 = 15 - 2 = 13$$

**Exercise B.** Compute $\det\begin{pmatrix}2 & -1 & 0 \\ 1 & 3 & -2 \\ 0 & 1 & 4\end{pmatrix}$ using Sarrus' rule.

$$= 2(3)(4) + (-1)(-2)(0) + (1)(1)(0) - (0)(3)(0) - (-1)(1)(4) - (-2)(1)(2)$$
$$= 24 + 0 + 0 - 0 + 4 + 4 = 32$$

**Exercise C.** Given $A$ is triangular: $A = \begin{pmatrix}3 & 5 & 7 \\ 0 & 2 & 4 \\ 0 & 0 & -1\end{pmatrix}$. Compute $\det(A)$.
$$\det(A) = 3 \times 2 \times (-1) = -6$$

---

## 9. Matrix Inverse via Cofactors

**Proposition:** If $A$ is invertible:
$$A^{-1} = \frac{C^T}{\det(A)}$$

where $C$ is the **cofactor matrix**: $c_{ij} = (-1)^{i+j} \det(A_{ij})$.

### Worked Example (from notes)

$$A = \begin{pmatrix}1 & 2 & 0 \\ 3 & 4 & 5 \\ 3 & 0 & 4\end{pmatrix}, \quad \det(A) = 22$$

Compute each cofactor $c_{ij} = (-1)^{i+j} \det(A_{ij})$:

$$c_{11} = +\det\begin{pmatrix}4&5\\0&4\end{pmatrix} = 16, \quad c_{12} = -\det\begin{pmatrix}3&5\\3&4\end{pmatrix} = 3, \quad c_{13} = +\det\begin{pmatrix}3&4\\3&0\end{pmatrix} = -12$$

$$c_{21} = -\det\begin{pmatrix}2&0\\0&4\end{pmatrix} = -8, \quad c_{22} = +\det\begin{pmatrix}1&0\\3&4\end{pmatrix} = 4, \quad c_{23} = -\det\begin{pmatrix}1&2\\3&0\end{pmatrix} = 6$$

$$c_{31} = +\det\begin{pmatrix}2&0\\4&5\end{pmatrix} = 10, \quad c_{32} = -\det\begin{pmatrix}1&0\\3&5\end{pmatrix} = -5, \quad c_{33} = +\det\begin{pmatrix}1&2\\3&4\end{pmatrix} = -2$$

$$C = \begin{pmatrix}16 & 3 & -12 \\ -8 & 4 & 6 \\ 10 & -5 & -2\end{pmatrix}, \quad A^{-1} = \frac{C^T}{22} = \begin{pmatrix}8/11 & -4/11 & 5/11 \\ 3/22 & 2/11 & -5/22 \\ -6/11 & 3/11 & -1/11\end{pmatrix}$$

### 3 Additional Exercises

**Exercise A.** Find $A^{-1}$ for $A = \begin{pmatrix}2 & 1 \\ 5 & 3\end{pmatrix}$.

$\det(A) = 6 - 5 = 1$.
$$A^{-1} = \frac{1}{1}\begin{pmatrix}3 & -1 \\ -5 & 2\end{pmatrix} = \begin{pmatrix}3 & -1 \\ -5 & 2\end{pmatrix}$$

Check: $A \cdot A^{-1} = \begin{pmatrix}6-5 & -2+2 \\ 15-15 & -5+6\end{pmatrix} = I$ ✓

**Exercise B.** Find $A^{-1}$ for $A = \begin{pmatrix}4 & 7 \\ 2 & 6\end{pmatrix}$.

$\det(A) = 24 - 14 = 10$.
$$A^{-1} = \frac{1}{10}\begin{pmatrix}6 & -7 \\ -2 & 4\end{pmatrix} = \begin{pmatrix}0.6 & -0.7 \\ -0.2 & 0.4\end{pmatrix}$$

**Exercise C.** Is $A = \begin{pmatrix}1 & 2 \\ 2 & 4\end{pmatrix}$ invertible? Why?

$\det(A) = 4 - 4 = 0$. **Not invertible (singular).** The second row is twice the first — linearly dependent columns.

---

## 10. Conditions for Invertibility

> ⚠️ **Exam Tip:** These five equivalent conditions for invertibility are frequently tested. Knowing all of them is essential.

For $A \in \mathbb{C}^{n \times n}$, the following are **equivalent**:

1. $A$ is **nonsingular** (invertible)
2. $\det(A) \neq 0$
3. $\text{rank}(A) = n$
4. The columns of $A$ are **linearly independent**
5. The rows of $A$ are **linearly independent**

**For orthogonal matrices:**
$$\det(A) = \pm 1$$

> 💡 **Additional Context (from assistant):** In AI/ML, singular matrices appear when your feature matrix has redundant features (multicollinearity). A singular covariance matrix means some features are perfectly correlated — this causes problems in algorithms like linear regression and PCA. The conditions above tell you exactly when this happens.

---

## 11. Eigenvalues and Eigenvectors

> ⚠️ **Exam Tip:** Eigenvalues and eigenvectors are central to many AI algorithms (PCA, spectral methods, SVD). These definitions and computation methods will almost certainly appear on the exam.

**Definition:** $\lambda \in \mathbb{C}$ is an **eigenvalue** of $A \in \mathbb{C}^{n \times n}$ if:
$$\exists\, x \in \mathbb{C}^n,\, x \neq \mathbf{0}: \quad Ax = \lambda x$$

$x$ is the corresponding **eigenvector**. The set of all eigenvalues is the **spectrum** $\sigma(A)$.

**Finding eigenvalues** — solve the **characteristic equation**:
$$p_A(\lambda) = \det(A - \lambda I) = 0$$

$p_A(\lambda)$ is the **characteristic polynomial** of degree $n$. By the fundamental theorem of algebra, $A$ has exactly $n$ eigenvalues (counted with multiplicity) over $\mathbb{C}$.

**Key propositions:**
- $\det(A) = \prod_{i=1}^n \lambda_i$ (determinant = product of eigenvalues)
- $A$ is **singular iff** at least one eigenvalue is zero
- If $A$ is **diagonal or triangular**: $\lambda_i = a_{ii}$
- If $A$ is **symmetric positive (semi)definite**: all eigenvalues $\lambda_i \geq 0$ (or $> 0$)
- Eigenvectors are **not unique**: if $x$ is an eigenvector, so is $cx$ for any $c \neq 0$

**Similar matrices:** $A$ and $B$ are **similar** if $\exists$ nonsingular $P$: $B = PAP^{-1}$.
Similar matrices have the **same eigenvalues**.

> 💡 **Additional Context (from assistant):** Geometrically, an eigenvector is a direction that $A$ only **scales** (by $\lambda$), not rotates. In PCA, the eigenvectors of the covariance matrix are the **principal components** — the directions of maximum variance in the data.

### 3 Additional Exercises

**Exercise A.** Find the eigenvalues of $A = \begin{pmatrix}3 & 1 \\ 1 & 3\end{pmatrix}$.

$$\det(A - \lambda I) = \det\begin{pmatrix}3-\lambda & 1 \\ 1 & 3-\lambda\end{pmatrix} = (3-\lambda)^2 - 1 = 0$$
$$\lambda^2 - 6\lambda + 8 = 0 \implies (\lambda - 4)(\lambda - 2) = 0$$
$$\lambda_1 = 4, \quad \lambda_2 = 2$$

**Exercise B.** Find eigenvectors for $\lambda_1 = 4$ from Exercise A.

$(A - 4I)x = 0$:
$$\begin{pmatrix}-1 & 1 \\ 1 & -1\end{pmatrix}\begin{pmatrix}x_1 \\ x_2\end{pmatrix} = 0 \implies -x_1 + x_2 = 0 \implies x_1 = x_2$$

Eigenvector: $x = \begin{pmatrix}1 \\ 1\end{pmatrix}$ (or any scalar multiple).

**Exercise C.** Find eigenvalues of the upper triangular matrix $B = \begin{pmatrix}5 & 3 \\ 0 & 2\end{pmatrix}$.

For triangular/diagonal matrices, eigenvalues are the diagonal entries: $\lambda_1 = 5$, $\lambda_2 = 2$. Verify: $\det(B - \lambda I) = (5-\lambda)(2-\lambda) = 0$.

---

## 12. Spectral Radius

**Definition:** The **spectral radius** of $A \in \mathbb{C}^{n \times n}$ is:
$$\rho(A) = \max_{\lambda \in \sigma(A)} |\lambda|$$

The spectral radius is the magnitude of the largest eigenvalue.

> 💡 **Additional Context (from assistant):** The spectral radius controls the convergence of iterative algorithms. If $\rho(A) < 1$, the iterative scheme $x_{k+1} = Ax_k$ converges to zero. This concept is critical in understanding convergence of gradient descent and other iterative solvers.

---

## 13. Norms on Vector Spaces

> ⚠️ **Exam Tip:** Know the 3 axioms of a norm and be able to compute $\ell^1$, $\ell^2$, and $\ell^\infty$ norms.

A **norm** on $V$ is a map $\|\cdot\| : V \to \mathbb{F}$ satisfying:
1. $\|v\| \geq 0$ and $\|v\| = 0 \iff v = \mathbf{0}$ (positive definiteness)
2. $\|\alpha v\| = |\alpha| \|v\|$ (homogeneity)
3. $\|v + w\| \leq \|v\| + \|w\|$ (triangle inequality)

### Common Vector Norms

For $v = (v_1, \ldots, v_n) \in \mathbb{R}^n$:

| Norm | Formula | Example: $v = (1,2,3)$ |
|---|---|---|
| **1-norm** $\|v\|_1$ | $\sum_{i=1}^n |v_i|$ | $1+2+3 = 6$ |
| **Euclidean (2-norm)** $\|v\|_2$ | $\sqrt{\sum_{i=1}^n v_i^2}$ | $\sqrt{1+4+9} = \sqrt{14}$ |
| **$p$-norm** $\|v\|_p$ | $\left(\sum_{i=1}^n |v_i|^p\right)^{1/p}$ | Generalizes above |
| **$\infty$-norm** $\|v\|_\infty$ | $\max_{i} |v_i|$ | $\max(1,2,3) = 3$ |

**Example:** For $v = (-8, 2, 5)$: $\|v\|_\infty = \max(8, 2, 5) = 8$

**Result:** In any finite-dimensional vector space, all $p$-norms are **equivalent** — they induce the same topology.

### 3 Additional Exercises

**Exercise A.** Compute $\|v\|_1$, $\|v\|_2$, $\|v\|_\infty$ for $v = (-3, 4, 0, -1)$.
- $\|v\|_1 = 3 + 4 + 0 + 1 = 8$
- $\|v\|_2 = \sqrt{9 + 16 + 0 + 1} = \sqrt{26} \approx 5.099$
- $\|v\|_\infty = \max(3, 4, 0, 1) = 4$

**Exercise B.** Compute $\|v\|_3$ for $v = (1, 2, 2)$.
$$\|v\|_3 = (|1|^3 + |2|^3 + |2|^3)^{1/3} = (1 + 8 + 8)^{1/3} = 17^{1/3} \approx 2.571$$

**Exercise C.** Verify the triangle inequality for $\|v+w\|_2$ with $v=(1,0)$, $w=(0,1)$.
$$\|v+w\|_2 = \|(1,1)\|_2 = \sqrt{2} \approx 1.414$$
$$\|v\|_2 + \|w\|_2 = 1 + 1 = 2 \geq 1.414 \quad ✓$$

---

## 14. Matrix Norms

A **matrix norm** $\|\cdot\| : \mathbb{R}^{m \times n} \to \mathbb{R}$ satisfies the same 3 axioms as vector norms.

A matrix norm is **compatible** with a vector norm if:
$$\|Ax\| \leq \|A\| \cdot \|x\|$$

### Common Matrix Norms

| Norm | Formula | Identity matrix |
|---|---|---|
| **Spectral norm** $\|A\|_2$ | $\sqrt{\rho(A^T A)}$ | $\|I\|_2 = 1$ |
| **1-norm** $\|A\|_1$ | $\max_j \sum_i |a_{ij}|$ (max column sum) | $\|I\|_1 = 1$ |
| **$\infty$-norm** $\|A\|_\infty$ | $\max_i \sum_j |a_{ij}|$ (max row sum) | $\|I\|_\infty = 1$ |
| **Frobenius norm** $\|A\|_F$ | $\sqrt{\sum_{i,j} |a_{ij}|^2}$ | $\|I_n\|_F = \sqrt{n}$ |

> ⚠️ **Exam Tip:** The 1-norm is the maximum **column** sum; the $\infty$-norm is the maximum **row** sum. Students often confuse these.

> 💡 **Additional Context (from assistant):** The Frobenius norm is essentially the Euclidean ($\ell^2$) norm applied to the matrix as a flat vector of all its entries. It appears in regularization (e.g., weight decay in neural networks: $\|W\|_F^2$ is minimized). The spectral norm $\|A\|_2$ gives the largest singular value of $A$ and controls how much $A$ can stretch a vector.

**Note:** If $A$ is symmetric: $\|A\|_1 = \|A\|_\infty$.

### 3 Additional Exercises

**Exercise A.** Compute $\|A\|_1$, $\|A\|_\infty$, $\|A\|_F$ for $A = \begin{pmatrix}1 & -2 \\ 3 & 4\end{pmatrix}$.

- $\|A\|_1 = \max(|1|+|3|, |-2|+|4|) = \max(4, 6) = 6$
- $\|A\|_\infty = \max(|1|+|-2|, |3|+|4|) = \max(3, 7) = 7$
- $\|A\|_F = \sqrt{1 + 4 + 9 + 16} = \sqrt{30} \approx 5.477$

**Exercise B.** Compute $\|I_3\|_F$.
$$\|I_3\|_F = \sqrt{1^2 + 1^2 + 1^2} = \sqrt{3}$$

**Exercise C.** For $A = \begin{pmatrix}2 & 0 \\ 0 & 3\end{pmatrix}$ (diagonal), verify $\|A\|_2 = \sqrt{\rho(A^T A)}$.

$A^T A = A^2 = \begin{pmatrix}4 & 0 \\ 0 & 9\end{pmatrix}$. Eigenvalues of $A^TA$: $4$ and $9$. So $\rho(A^TA) = 9$.
$$\|A\|_2 = \sqrt{9} = 3 = \max(|2|, |3|)$$

For diagonal matrices, the spectral norm equals the largest absolute diagonal entry. ✓

---

## 📝 Review Questions (30 Questions)

---

**Q1. ⚠️ State the 8 axioms of a vector space.**

**Answer:** Commutativity and associativity of addition; existence of additive identity (zero vector) and additive inverse; multiplicative identity scalar (1); compatibility of scalar multiplication with field multiplication; two distributivity laws (over field addition and over vector addition).

---

**Q2. Give 4 examples of vector spaces and state their fields.**

**Answer:**
1. $\mathbb{R}^n$ over $\mathbb{R}$
2. $\mathbb{C}^n$ over $\mathbb{C}$
3. $\mathcal{P}_n$ (polynomials of degree $\leq n$) over $\mathbb{R}$
4. $\mathcal{C}^n([a,b])$ (continuously differentiable functions) over $\mathbb{R}$

---

**Q3. ⚠️ What are the conditions for a subset $W \subset V$ to be a subspace?**

**Answer:** $W$ must be closed under addition ($w_1 + w_2 \in W$ for all $w_1, w_2 \in W$) and under scalar multiplication ($\alpha w \in W$ for all $\alpha \in \mathbb{F}$, $w \in W$). Equivalently, $W$ must be a vector space over $\mathbb{F}$ contained in $V$.

---

**Q4. Is $W = \{(x, y) \in \mathbb{R}^2 : x + y = 1\}$ a subspace of $\mathbb{R}^2$?**

**Answer:** No. The zero vector $(0,0)$ does not satisfy $0+0=1$, so the additive identity is missing. Also, $(1,0) + (0,1) = (1,1)$ and $1+1 = 2 \neq 1$, so closure under addition fails.

---

**Q5. ⚠️ Define linear independence. How do you test it?**

**Answer:** $\{v_1, \ldots, v_m\}$ is linearly independent if $\sum \alpha_i v_i = 0 \implies \alpha_i = 0$ for all $i$. Test: set up the linear system and check if the only solution is the trivial one (all $\alpha_i = 0$). Equivalently, compute the determinant of the matrix formed by these vectors — if $\det \neq 0$, they are independent.

---

**Q6. Show that $v_1 = (1,2)$ and $v_2 = (2,4)$ in $\mathbb{R}^2$ are linearly dependent.**

**Answer:**
$\alpha_1(1,2) + \alpha_2(2,4) = (0,0) \implies \alpha_1 + 2\alpha_2 = 0$ and $2\alpha_1 + 4\alpha_2 = 0$.

The second equation is twice the first — infinitely many solutions exist (e.g., $\alpha_1 = 2, \alpha_2 = -1$). Therefore $v_1$ and $v_2$ are **linearly dependent** ($v_2 = 2v_1$).

---

**Q7. ⚠️ Define a basis and the dimension of a vector space.**

**Answer:** A **basis** is a linearly independent set that spans $V$. The **dimension** $\dim(V) = n$ is the number of vectors in any basis. Every basis of $V$ has the same number of elements ($n$).

---

**Q8. What is the standard basis for $\mathbb{R}^3$? What is $\dim(\mathbb{R}^3)$?**

**Answer:** Standard basis: $\{e_1, e_2, e_3\} = \{(1,0,0), (0,1,0), (0,0,1)\}$. $\dim(\mathbb{R}^3) = 3$.

---

**Q9. What is the rank of a matrix? When is it full rank?**

**Answer:** $\text{rank}(A)$ is the maximum number of linearly independent columns (or equivalently rows) of $A$. $A$ has **full rank** when $\text{rank}(A) = \min(m,n)$.

---

**Q10. ⚠️ What is the difference between a lower and upper triangular matrix?**

**Answer:** Lower triangular: all entries **above** the diagonal are zero ($l_{ij} = 0$ for $i < j$). Upper triangular: all entries **below** the diagonal are zero ($u_{ij} = 0$ for $i > j$).

---

**Q11. Compute $AB$ for $A = \begin{pmatrix}1&0\\2&1\end{pmatrix}$, $B = \begin{pmatrix}3\\4\end{pmatrix}$.**

**Answer:**
$$AB = \begin{pmatrix}1\cdot3+0\cdot4\\2\cdot3+1\cdot4\end{pmatrix} = \begin{pmatrix}3\\10\end{pmatrix}$$

---

**Q12. ⚠️ Prove $(AB)^T = B^T A^T$ for a $2 \times 2$ example.**

**Answer:** Let $A = \begin{pmatrix}a&b\\c&d\end{pmatrix}$, $B = \begin{pmatrix}e&f\\g&h\end{pmatrix}$.

$AB = \begin{pmatrix}ae+bg & af+bh \\ ce+dg & cf+dh\end{pmatrix}$, $(AB)^T = \begin{pmatrix}ae+bg & ce+dg \\ af+bh & cf+dh\end{pmatrix}$

$B^T A^T = \begin{pmatrix}e&g\\f&h\end{pmatrix}\begin{pmatrix}a&c\\b&d\end{pmatrix} = \begin{pmatrix}ea+gb & ec+gd \\ fa+hb & fc+hd\end{pmatrix}$ ✓ (identical)

---

**Q13. Define: diagonal, identity, symmetric, antisymmetric, orthogonal matrices.**

**Answer:**
- **Diagonal:** Non-zero only on main diagonal
- **Identity:** Diagonal with all 1s; $AI = IA = A$
- **Symmetric:** $A = A^T$
- **Antisymmetric:** $A = -A^T$ (diagonal entries = 0)
- **Orthogonal:** $A^{-1} = A^T$, so $AA^T = I$

---

**Q14. ⚠️ Compute $\det\begin{pmatrix}2&3\\1&4\end{pmatrix}$.**

**Answer:** $\det = 2 \cdot 4 - 3 \cdot 1 = 8 - 3 = 5$.

---

**Q15. ⚠️ Use Sarrus' rule to compute $\det\begin{pmatrix}1&2&3\\0&1&4\\5&6&0\end{pmatrix}$.**

**Answer:**
$$\det = 1(1)(0) + 2(4)(5) + 0(6)(3) - 3(1)(5) - 2(0)(0) - 4(6)(1)$$
$$= 0 + 40 + 0 - 15 - 0 - 24 = 1$$

---

**Q16. State all 5 equivalent conditions for a matrix to be invertible.**

**Answer:** (1) Nonsingular ($A^{-1}$ exists). (2) $\det(A) \neq 0$. (3) $\text{rank}(A) = n$. (4) Linearly independent columns. (5) Linearly independent rows. All are equivalent for square $A \in \mathbb{C}^{n\times n}$.

---

**Q17. Find $A^{-1}$ for $A = \begin{pmatrix}3&4\\2&3\end{pmatrix}$.**

**Answer:** $\det(A) = 9 - 8 = 1$. $A^{-1} = \frac{1}{1}\begin{pmatrix}3&-4\\-2&3\end{pmatrix} = \begin{pmatrix}3&-4\\-2&3\end{pmatrix}$.

Verify: $\begin{pmatrix}3&4\\2&3\end{pmatrix}\begin{pmatrix}3&-4\\-2&3\end{pmatrix} = \begin{pmatrix}9-8&-12+12\\6-6&-8+9\end{pmatrix} = I$ ✓

---

**Q18. ⚠️ Define eigenvalue and eigenvector. How do you find them?**

**Answer:** $\lambda$ is an eigenvalue of $A$ if $\exists x \neq 0$: $Ax = \lambda x$. $x$ is the eigenvector. Find eigenvalues by solving $\det(A - \lambda I) = 0$ (characteristic equation). Then for each $\lambda$, find eigenvectors by solving $(A - \lambda I)x = 0$.

---

**Q19. Find all eigenvalues of $A = \begin{pmatrix}4 & 1 \\ 2 & 3\end{pmatrix}$.**

**Answer:**
$$\det(A - \lambda I) = (4-\lambda)(3-\lambda) - 2 = \lambda^2 - 7\lambda + 10 = 0$$
$$(\lambda-5)(\lambda-2) = 0 \implies \lambda_1 = 5, \lambda_2 = 2$$

Verify: $\det(A) = 12 - 2 = 10 = \lambda_1 \lambda_2 = 5 \cdot 2$ ✓

---

**Q20. Find the eigenvector for $\lambda = 5$ in Q19.**

**Answer:**
$(A - 5I)x = 0$: $\begin{pmatrix}-1 & 1 \\ 2 & -2\end{pmatrix}x = 0 \implies -x_1 + x_2 = 0 \implies x_1 = x_2$

Eigenvector: $x = \begin{pmatrix}1 \\ 1\end{pmatrix}$.

---

**Q21. What is the spectral radius? Why is it important in AI?**

**Answer:** $\rho(A) = \max_{\lambda \in \sigma(A)} |\lambda|$. It is the largest absolute eigenvalue. In AI, it controls the convergence of iterative algorithms — if $\rho < 1$, repeated application of $A$ contracts toward zero (convergent). It also determines the condition number of linear systems.

---

**Q22. ⚠️ State the 3 axioms of a norm.**

**Answer:**
1. $\|v\| \geq 0$ and $\|v\| = 0 \iff v = \mathbf{0}$
2. $\|\alpha v\| = |\alpha| \|v\|$
3. $\|v + w\| \leq \|v\| + \|w\|$ (triangle inequality)

---

**Q23. Compute $\|v\|_1$, $\|v\|_2$, $\|v\|_\infty$ for $v = (3, -4, 0)$.**

**Answer:**
- $\|v\|_1 = 3 + 4 + 0 = 7$
- $\|v\|_2 = \sqrt{9 + 16 + 0} = \sqrt{25} = 5$
- $\|v\|_\infty = \max(3, 4, 0) = 4$

---

**Q24. What is the $p$-norm? What does it reduce to for $p=1$ and $p=2$?**

**Answer:** $\|v\|_p = \left(\sum_{i=1}^n |v_i|^p\right)^{1/p}$. For $p=1$: 1-norm (sum of absolute values). For $p=2$: Euclidean norm (square root of sum of squares).

---

**Q25. ⚠️ State the matrix 1-norm and $\infty$-norm formulas. Which uses row sums and which uses column sums?**

**Answer:**
- $\|A\|_1 = \max_j \sum_i |a_{ij}|$ — maximum **column** absolute sum
- $\|A\|_\infty = \max_i \sum_j |a_{ij}|$ — maximum **row** absolute sum

Memory tip: the subscript 1 → **c**o**l**umns (think "column"), $\infty$ → rows.

---

**Q26. Compute $\|A\|_1$, $\|A\|_\infty$, $\|A\|_F$ for $A = \begin{pmatrix}2 & -3 \\ 1 & 4\end{pmatrix}$.**

**Answer:**
- $\|A\|_1 = \max(|2|+|1|, |-3|+|4|) = \max(3, 7) = 7$
- $\|A\|_\infty = \max(|2|+|-3|, |1|+|4|) = \max(5, 5) = 5$
- $\|A\|_F = \sqrt{4 + 9 + 1 + 16} = \sqrt{30} \approx 5.48$

---

**Q27. What is the spectral norm $\|A\|_2$? How does it relate to eigenvalues?**

**Answer:** $\|A\|_2 = \sqrt{\rho(A^T A)}$, the square root of the largest eigenvalue of $A^T A$. It equals the largest **singular value** of $A$. For symmetric positive definite $A$, $\|A\|_2 = \rho(A)$ (the largest eigenvalue).

---

**Q28. Show that $\|I_n\|_F = \sqrt{n}$.**

**Answer:** The identity matrix $I_n$ has exactly $n$ non-zero entries, all equal to 1 (on the diagonal). Therefore:
$$\|I_n\|_F = \sqrt{\sum_{i,j} |a_{ij}|^2} = \sqrt{\underbrace{1^2 + 1^2 + \cdots + 1^2}_{n \text{ terms}}} = \sqrt{n}$$

---

**Q29. ⚠️ What is the relationship between eigenvectors and linear independence?**

**Answer:** Eigenvectors corresponding to **distinct eigenvalues** are always linearly independent. This is important because a matrix with $n$ distinct eigenvalues is always diagonalizable — it has $n$ linearly independent eigenvectors that form a basis.

---

**Q30. ⚠️ Summarize the chain of concepts: vector space → basis → matrix → eigenvalues → norms, and explain how each connects to AI.**

**Answer:**
- **Vector spaces** provide the mathematical setting for feature spaces, weight spaces, and function spaces in ML.
- **Bases** give us coordinate representations — the dimension of the feature space is the number of basis vectors.
- **Matrices** represent linear transformations — neural network layers, covariance matrices, and kernel functions are all matrices.
- **Eigenvalues/eigenvectors** reveal the fundamental structure of a matrix — used in PCA (directions of variance), stability analysis, and spectral graph methods.
- **Norms** measure distances and magnitudes — used in loss functions ($\ell^2$ = MSE, $\ell^1$ = MAE), regularization (weight decay, Lasso), and convergence criteria.

All of these form the mathematical backbone of every major AI algorithm.
