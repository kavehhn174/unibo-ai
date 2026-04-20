# MML Chapter 5: Vector Calculus — Study Notes

### Course: Statistical and Mathematical Methods for Artificial Intelligence | University of Bologna

---

## Table of Contents

1. [Partial Derivatives and the Jacobian](#1-partial-derivatives-and-the-jacobian)
2. [Gradients](#2-gradients)
3. [Chain Rule](#3-chain-rule)
4. [The Jacobian Matrix (General Case)](#4-the-jacobian-matrix-general-case)
5. [Gradients of Vector-Valued Functions](#5-gradients-of-vector-valued-functions)
6. [Least-Squares Loss and Gradient Derivation](#6-least-squares-loss-and-gradient-derivation)
7. [Hessian Matrix](#7-hessian-matrix)
8. [Linearization and Multivariate Taylor Series](#8-linearization-and-multivariate-taylor-series)
9. [Backpropagation in Neural Networks](#9-backpropagation-in-neural-networks)
10. [Further Reading and Applications](#10-further-reading-and-applications)
11. [Exercises with Full Solutions](#11-exercises-with-full-solutions)
12. [📝 Review Questions (30 Questions)](#-review-questions-30-questions)

---

## 1. Partial Derivatives and the Jacobian

### 1.1 Partial Derivatives

For a function $f: \mathbb{R}^n \to \mathbb{R}$, the **partial derivative** with respect to $x_i$ is:

$$\frac{\partial f}{\partial x_i} = \lim_{h \to 0} \frac{f(x_1, \ldots, x_i + h, \ldots, x_n) - f(x_1, \ldots, x_i, \ldots, x_n)}{h}$$

> ⚠️ **Exam Tip:** When computing a partial derivative with respect to $x_i$, treat all other variables as constants.

🧭 **Exam Priority: HIGH** — partial derivatives are foundational and frequently tested.

**Example:** Given $f(x_1, x_2) = x_1^2 + x_1 x_2$:

$$\frac{\partial f}{\partial x_1} = 2x_1 + x_2, \quad \frac{\partial f}{\partial x_2} = x_1$$

**Additional Examples:**

1. $f(x_1, x_2) = x_1^3 + 2x_1 x_2 - x_2^2$:
   - $\frac{\partial f}{\partial x_1} = 3x_1^2 + 2x_2$
   - $\frac{\partial f}{\partial x_2} = 2x_1 - 2x_2$

2. $f(x_1, x_2) = e^{x_1} \sin(x_2)$:
   - $\frac{\partial f}{\partial x_1} = e^{x_1} \sin(x_2)$
   - $\frac{\partial f}{\partial x_2} = e^{x_1} \cos(x_2)$

3. $f(x_1, x_2) = x_1^2 x_2 + \ln(x_2)$:
   - $\frac{\partial f}{\partial x_1} = 2x_1 x_2$
   - $\frac{\partial f}{\partial x_2} = x_1^2 + \frac{1}{x_2}$

---

## 2. Gradients

For $f: \mathbb{R}^n \to \mathbb{R}$, the **gradient** is a row vector:

$$\nabla_x f = \left[ \frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \ldots, \frac{\partial f}{\partial x_n} \right] \in \mathbb{R}^{1 \times n}$$

> ⚠️ **Exam Tip:** The gradient points in the direction of steepest ascent of $f$. This is crucial for optimization (gradient descent moves opposite to the gradient).

🧭 **Exam Priority: HIGH** — know how to compute a gradient and evaluate it at a point.

> 💡 **Additional Context (from assistant):** Think of the gradient like a "slope compass" in multiple dimensions. In 2D, you might have a hill — the gradient at any point tells you in which direction the hill rises most steeply.

**Example:** $f(x_1, x_2) = x_1^2 + x_1 x_2$

$$\nabla f = \left[ 2x_1 + x_2, \quad x_1 \right]$$

At point $(1, 2)$: $\nabla f(1,2) = [4, 1]$

**Additional Examples:**

1. $f(x_1, x_2) = 3x_1^2 - x_2^3$:
   - $\nabla f = [6x_1, -3x_2^2]$
   - At $(1, 1)$: $\nabla f = [6, -3]$

2. $f(x_1, x_2) = e^{x_1 + x_2}$:
   - $\nabla f = [e^{x_1+x_2}, e^{x_1+x_2}]$
   - At $(0,0)$: $\nabla f = [1, 1]$

3. $f(x_1, x_2, x_3) = x_1 x_2 + x_2 x_3 + x_3 x_1$:
   - $\nabla f = [x_2 + x_3, x_1 + x_3, x_2 + x_1]$

---

## 3. Chain Rule

### 3.1 Univariate Chain Rule

For composed functions $f \circ \mathbf{x}$ where $f: \mathbb{R}^n \to \mathbb{R}$ and $\mathbf{x}: \mathbb{R} \to \mathbb{R}^n$:

$$\frac{d(f \circ \mathbf{x})}{dt} = \frac{\partial f}{\partial x_1} \frac{dx_1}{dt} + \frac{\partial f}{\partial x_2} \frac{dx_2}{dt} = \nabla_x f \cdot \dot{\mathbf{x}}(t)$$

> ⚠️ **Exam Tip:** The chain rule is the backbone of backpropagation in neural networks. Know this cold.

🧭 **Exam Priority: HIGH** — chain-rule computations often appear in both calculus and ML-style questions.

**Example:** $f(x_1, x_2) = x_1^2 + 2x_2$, with $\mathbf{x}(t) = (\sin(t), \cos(t))$:

$$\frac{d(f \circ \mathbf{x})}{dt} = 2x_1 \cdot \cos(t) + 2 \cdot (-\sin(t))$$
$$= 2\sin(t)\cos(t) - 2\sin(t)$$

**Additional Examples:**

1. $f(x_1, x_2) = x_1 x_2$, $\mathbf{x}(t) = (t^2, e^t)$:
   - $\frac{\partial f}{\partial x_1} = x_2 = e^t$, $\frac{\partial f}{\partial x_2} = x_1 = t^2$
   - $\dot{x}_1 = 2t$, $\dot{x}_2 = e^t$
   - $\frac{d(f \circ \mathbf{x})}{dt} = e^t \cdot 2t + t^2 \cdot e^t = e^t(2t + t^2)$

2. $f(x_1, x_2) = x_1^2 + x_2^2$, $\mathbf{x}(t) = (\cos(t), \sin(t))$:
   - $\frac{d(f \circ \mathbf{x})}{dt} = 2\cos(t)(-\sin(t)) + 2\sin(t)\cos(t) = 0$
   - (Makes sense — $f = 1$ is constant on the unit circle!)

3. $f(x_1, x_2) = x_1^3 - x_2$, $\mathbf{x}(t) = (t, t^2)$:
   - $\frac{d(f \circ \mathbf{x})}{dt} = 3x_1^2 \cdot 1 + (-1) \cdot 2t = 3t^2 - 2t$

### 3.2 Multivariate Chain Rule

For $f: \mathbb{R}^n \to \mathbb{R}$ and $\mathbf{x}: \mathbb{R}^m \to \mathbb{R}^n$ (so $\mathbf{x} = \mathbf{x}(s, t)$):

$$\frac{\partial(f \circ \mathbf{x})}{\partial(s,t)} = \frac{\partial f}{\partial \mathbf{x}} \cdot \frac{\partial \mathbf{x}}{\partial(s,t)} = \underbrace{\nabla_\mathbf{x} f}_{1 \times n} \cdot \underbrace{J_\mathbf{x}}_{n \times m}$$

**Example:** $f(x_1, x_2) = x_1 + 2x_2$, $\mathbf{x}(s,t) = (t\cos(s),\ s\sinh(t))$:

$$\nabla f = [1, 2], \quad J_\mathbf{x} = \begin{bmatrix} -t\sin(s) & \cos(s) \\ \sinh(t) & s\cosh(t) \end{bmatrix}$$

$$\frac{\partial(f \circ \mathbf{x})}{\partial(s,t)} = [1, 2] \cdot J_\mathbf{x} = [-t\sin(s) + 2\sinh(t),\ \cos(s) + 2s\cosh(t)]$$

---

## 4. The Jacobian Matrix (General Case)

For $f: \mathbb{R}^n \to \mathbb{R}^m$, the **Jacobian** is an $m \times n$ matrix:

$$J = \frac{\partial \mathbf{f}}{\partial \mathbf{x}} = \begin{bmatrix}
\frac{\partial f_1}{\partial x_1} & \cdots & \frac{\partial f_1}{\partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial f_m}{\partial x_1} & \cdots & \frac{\partial f_m}{\partial x_n}
\end{bmatrix} \in \mathbb{R}^{m \times n}$$

> ⚠️ **Exam Tip:** The Jacobian generalizes the derivative to vector-valued functions. Know its dimensions: $m \times n$ (outputs × inputs).

🧭 **Exam Priority: HIGH** — dimensions and construction of the Jacobian are common exam targets.

**Example:** $f: \mathbb{R}^2 \to \mathbb{R}^3$ with

$$f(x_1, x_2) = \begin{bmatrix} 2x_2 + x_1 \\ x_2 \\ x_1 \end{bmatrix}$$

$$J = \begin{bmatrix} 1 & 2 \\ 0 & 1 \\ 1 & 0 \end{bmatrix} \in \mathbb{R}^{3 \times 2}$$

> 💡 **Additional Context (from assistant):** The Jacobian evaluated at a point gives the best linear approximation of $f$ at that point. If $f$ is a neural network layer, the Jacobian tells you how sensitive each output is to each input — essential for backpropagation.

**Additional Examples:**

1. $f: \mathbb{R}^2 \to \mathbb{R}^2$, $f(x_1, x_2) = (x_1^2 + x_2,\ x_1 x_2)$:

$$J = \begin{bmatrix} 2x_1 & 1 \\ x_2 & x_1 \end{bmatrix}$$

At $(1, 2)$: $J = \begin{bmatrix} 2 & 1 \\ 2 & 1 \end{bmatrix}$

2. $f: \mathbb{R}^3 \to \mathbb{R}^2$, $f(x) = (x_1 + x_2 + x_3,\ x_1^2 + x_3)$:

$$J = \begin{bmatrix} 1 & 1 & 1 \\ 2x_1 & 0 & 1 \end{bmatrix}$$

3. $f: \mathbb{R}^2 \to \mathbb{R}^2$, $f(r, \theta) = (r\cos\theta, r\sin\theta)$ (polar to Cartesian):

$$J = \begin{bmatrix} \cos\theta & -r\sin\theta \\ \sin\theta & r\cos\theta \end{bmatrix}$$

The determinant $|J| = r$ — this is why polar integration has a factor of $r$!

---

## 5. Gradients of Vector-Valued Functions

For the general chain rule with $f: \mathbb{R}^n \to \mathbb{R}^m$ and $g: \mathbb{R}^p \to \mathbb{R}^n$:

$$\frac{\partial(f \circ g)}{\partial \mathbf{x}} = \underbrace{\frac{\partial f}{\partial g}}_{m \times n} \cdot \underbrace{\frac{\partial g}{\partial \mathbf{x}}}_{n \times p} \in \mathbb{R}^{m \times p}$$

> ⚠️ **Exam Tip:** Always check matrix dimensions match when applying the chain rule in composed functions. The middle dimension must cancel.

🧭 **Exam Priority: HIGH** — a fast exam check is verifying dimension consistency before multiplying.

---

## 6. Least-Squares Loss and Gradient Derivation

This section covers a classic machine learning application of the Jacobian.

**Setup:** Given $\Phi \in \mathbb{R}^{N \times D}$ (feature matrix), $y \in \mathbb{R}^N$ (targets), $\theta \in \mathbb{R}^D$ (parameters):

- Prediction: $g(\theta) = \Phi\theta \in \mathbb{R}^N$
- Error: $e(\theta) = y - g(\theta) = y - \Phi\theta \in \mathbb{R}^N$
- Loss (squared): $L(e) = \|e\|^2 = e^\top e$

**Gradient of the Least-Squares Loss:**

Using the chain rule $\nabla(L \circ e)(\theta) = \frac{\partial L}{\partial e} \cdot \frac{\partial e}{\partial \theta}$:

$$\frac{\partial L}{\partial e} = 2e^\top \in \mathbb{R}^{1 \times N}$$

$$\frac{\partial e}{\partial \theta} = -\Phi \in \mathbb{R}^{N \times D}$$

$$\nabla_\theta L = \frac{\partial L}{\partial e} \cdot \frac{\partial e}{\partial \theta} = 2e^\top \cdot (-\Phi) = -2e^\top \Phi = -2(y - \Phi\theta)^\top \Phi$$

$$= -2y^\top \Phi + 2\theta^\top \Phi^\top \Phi$$

> ⚠️ **Exam Tip:** This derivation is often asked in exams. The final gradient $\nabla_\theta L = 2(\Phi^\top \Phi \theta - \Phi^\top y)$ leads to the **normal equations** $\Phi^\top \Phi \theta = \Phi^\top y$ when set to zero.

🧭 **Exam Priority: HIGH** — this derivation directly connects to normal equations and closed-form least squares.

> 💡 **Additional Context (from assistant):** Setting $\nabla_\theta L = 0$ gives the closed-form ordinary least-squares solution $\hat{\theta} = (\Phi^\top \Phi)^{-1} \Phi^\top y$, assuming $\Phi^\top \Phi$ is invertible.

**Additional Examples:**

1. **Scalar case:** $L(\theta) = (y - \theta)^2$, $\frac{dL}{d\theta} = -2(y - \theta)$. Set to 0: $\hat\theta = y$. ✓

2. **Ridge regression:** $L(\theta) = \|y - \Phi\theta\|^2 + \lambda\|\theta\|^2$:
   - $\nabla_\theta L = -2\Phi^\top(y - \Phi\theta) + 2\lambda\theta$
   - Solution: $\hat\theta = (\Phi^\top\Phi + \lambda I)^{-1}\Phi^\top y$

3. **Two-parameter case:** $L(w, b) = (y - wx - b)^2$:
   - $\frac{\partial L}{\partial w} = -2x(y - wx - b)$
   - $\frac{\partial L}{\partial b} = -2(y - wx - b)$

---

## 7. Hessian Matrix

For $f: \mathbb{R}^n \to \mathbb{R}$, the **Hessian** collects all second-order partial derivatives:

$$H = \nabla^2 f = \begin{bmatrix}
\frac{\partial^2 f}{\partial x_1^2} & \frac{\partial^2 f}{\partial x_1 \partial x_2} & \cdots \\
\frac{\partial^2 f}{\partial x_2 \partial x_1} & \frac{\partial^2 f}{\partial x_2^2} & \cdots \\
\vdots & \vdots & \ddots
\end{bmatrix} \in \mathbb{R}^{n \times n}$$

**Key Properties:**
- If $f$ is twice continuously differentiable, $H$ is **symmetric**: $\frac{\partial^2 f}{\partial x_i \partial x_j} = \frac{\partial^2 f}{\partial x_j \partial x_i}$
- The Hessian measures **local curvature** of the function
- For $f: \mathbb{R}^n \to \mathbb{R}^m$, the Hessian is an $(m \times n \times n)$-tensor

> ⚠️ **Exam Tip:** The Hessian is used in second-order optimization methods (Newton's method) and in determining whether a critical point is a minimum, maximum, or saddle point:
> - $H$ positive definite → local minimum
> - $H$ negative definite → local maximum
> - $H$ indefinite → saddle point

🧭 **Exam Priority: HIGH** — classification via Hessian definiteness is a classic exam question.

**Example:** $f(x, y) = x^2 + 2xy + y^3$:

$$H = \begin{bmatrix} 2 & 2 \\ 2 & 6y \end{bmatrix}$$

At $(1, 2)$: $H(1,2) = \begin{bmatrix} 2 & 2 \\ 2 & 12 \end{bmatrix}$

**Additional Examples:**

1. $f(x, y) = x^3 - 3xy + y^2$:

$$H = \begin{bmatrix} 6x & -3 \\ -3 & 2 \end{bmatrix}$$

2. $f(x, y, z) = x^2 + y^2 + z^2 + xy$:

$$H = \begin{bmatrix} 2 & 1 & 0 \\ 1 & 2 & 0 \\ 0 & 0 & 2 \end{bmatrix}$$

(Positive definite → any critical point is a minimum)

3. $f(x, y) = xy$:

$$H = \begin{bmatrix} 0 & 1 \\ 1 & 0 \end{bmatrix}$$

(Indefinite → saddle point)

---

## 8. Linearization and Multivariate Taylor Series

### 8.1 Linear Approximation

The gradient provides a **linear approximation** (first-order Taylor expansion) of $f$ around $x_0$:

$$f(x) \approx f(x_0) + (\nabla_x f)(x_0)(x - x_0)$$

![Linear approximation of a function using first-order Taylor series](https://images/slide_linearization_figure_5_12_taylor_linear_approx.png)
> 📌 **Note:** This image is located on **Slide 5.12** of the provided slides (figure title: "Linear approximation of a function"). Please place the file as `images/slide_linearization_figure_5_12_taylor_linear_approx.png` in the same GitHub repository — it will load automatically once added.

### 8.2 Multivariate Taylor Series (Definition 5.7)

For a smooth function $f: \mathbb{R}^D \to \mathbb{R}$, defining the difference vector $\delta := x - x_0$:

$$f(x) = \sum_{k=0}^\infty \frac{D_x^k f(x_0)}{k!} \delta^k$$

where $D_x^k f(x_0)$ is the $k$-th total derivative of $f$ at $x_0$.

### 8.3 Taylor Polynomial (Definition 5.8)

The **Taylor polynomial of degree $n$** keeps the first $n+1$ terms:

$$T_n(x) = \sum_{k=0}^n \frac{D_x^k f(x_0)}{k!} \delta^k$$

### 8.4 Explicit Terms

For $k = 0, 1, 2, 3$ and $\delta = x - x_0$:

| Order $k$ | Term | Meaning |
|-----------|------|---------|
| 0 | $f(x_0)$ | Constant — function value |
| 1 | $\nabla_x f(x_0) \cdot \delta$ | Linear — gradient |
| 2 | $\frac{1}{2}\delta^\top H(x_0) \delta$ | Quadratic — curvature |
| 3 | $\frac{1}{6}\sum_{i,j,k} D^3 f[i,j,k]\delta[i]\delta[j]\delta[k]$ | Cubic term |

> ⚠️ **Exam Tip:** The second-order term involves the **Hessian**: $\frac{1}{2}\delta^\top H \delta$. This is the most tested term after the linear one.

🧭 **Exam Priority: HIGH** — remember the exact second-order term form.

### 8.5 Outer Products and Tensor Notation

The $k$-fold outer product $\delta^k = \underbrace{\delta \otimes \cdots \otimes \delta}_{k}$ has dimensions $\mathbb{R}^{D \times D \times \cdots \times D}$ ($k$ times).

- $\delta^2 = \delta\delta^\top \in \mathbb{R}^{D \times D}$: a matrix
- $\delta^3 \in \mathbb{R}^{D \times D \times D}$: a third-order tensor

🧭 **Exam Priority: LOW** — useful for deeper understanding, but usually less tested than gradient/Jacobian/Hessian basics.

![Outer product visualization: (a) two vectors → matrix; (b) three vectors → tensor](https://images/slide_ch5_figure_5_13_outer_products.png)
> 📌 **Note:** This image is located on **Slide 5.13** of the provided slides (figure title: "Visualizing outer products"). Please place the file as `images/slide_ch5_figure_5_13_outer_products.png` in the same GitHub repository — it will load automatically once added.

### 8.6 Worked Example: Taylor Expansion of $f(x,y) = x^2 + 2xy + y^3$ at $(1,2)$

**Step 1 — Constant term ($k=0$):**
$$f(1, 2) = 1 + 4 + 8 = 13$$

**Step 2 — First-order term ($k=1$):**
$$\frac{\partial f}{\partial x} = 2x + 2y \Rightarrow \frac{\partial f}{\partial x}(1,2) = 6$$
$$\frac{\partial f}{\partial y} = 2x + 3y^2 \Rightarrow \frac{\partial f}{\partial y}(1,2) = 14$$
$$\nabla f(1,2) = [6,\ 14]$$
$$\text{Term}_1 = 6(x-1) + 14(y-2)$$

**Step 3 — Second-order term ($k=2$):**
$$H = \begin{bmatrix} 2 & 2 \\ 2 & 6y \end{bmatrix}, \quad H(1,2) = \begin{bmatrix} 2 & 2 \\ 2 & 12 \end{bmatrix}$$

$$\text{Term}_2 = \frac{1}{2}[x-1,\ y-2]\begin{bmatrix}2 & 2\\2 & 12\end{bmatrix}\begin{bmatrix}x-1\\y-2\end{bmatrix}$$
$$= (x-1)^2 + 2(x-1)(y-2) + 6(y-2)^2$$

**Step 4 — Third-order term ($k=3$):**

Only nonzero third derivative: $\frac{\partial^3 f}{\partial y^3} = 6$

$$\text{Term}_3 = \frac{6}{6}(y-2)^3 = (y-2)^3$$

**Full expansion:**
$$f(x,y) = 13 + 6(x-1) + 14(y-2) + (x-1)^2 + 2(x-1)(y-2) + 6(y-2)^2 + (y-2)^3$$

> ⚠️ **Exam Tip:** For a degree-$d$ polynomial, the Taylor series is exact at order $d$ — no approximation. This is a key insight to mention in exams.

🧭 **Exam Priority: MEDIUM-HIGH** — often asked as a short conceptual question.

**Additional Examples:**

**Example A:** $f(x) = \sin(x)$ at $x_0 = 0$, Taylor polynomial of degree 5:
- $f(0) = 0$, $f'(0) = 1$, $f''(0) = 0$, $f'''(0) = -1$, $f^{(4)}(0) = 0$, $f^{(5)}(0) = 1$
$$T_5(x) = x - \frac{x^3}{6} + \frac{x^5}{120}$$

**Example B:** $f(x,y) = e^{x+y}$ at $(0,0)$:
- $f(0,0) = 1$, $\nabla f = [1, 1]$, $H = \begin{bmatrix}1 & 1\\1 & 1\end{bmatrix}$
$$T_2(x,y) = 1 + x + y + \frac{1}{2}(x^2 + 2xy + y^2) = 1 + x + y + \frac{(x+y)^2}{2}$$

**Example C:** $f(x,y) = x^2 y$ at $(1, 1)$:
- $f(1,1) = 1$
- $\frac{\partial f}{\partial x} = 2xy \Rightarrow 2$; $\frac{\partial f}{\partial y} = x^2 \Rightarrow 1$
- $H = \begin{bmatrix} 2y & 2x \\ 2x & 0 \end{bmatrix}$ at $(1,1)$: $H = \begin{bmatrix}2 & 2\\2 & 0\end{bmatrix}$
$$T_2(x,y) = 1 + 2(x-1) + (y-1) + (x-1)^2 + 2(x-1)(y-1)$$

---

## 9. Backpropagation in Neural Networks

This section demonstrates the chain rule applied to compute gradients in a 2-layer neural network.

### 9.1 Architecture

- **Input:** $x \in \mathbb{R}$
- **First layer:** $z^{(1)} = w^{(1)} x + b^{(1)}$, then $h = \text{ReLU}(z^{(1)})$
- **Second layer:** $\hat{y} = w^{(2)} h + b^{(2)}$
- **Loss (MSE):** $L = \frac{1}{2}(\hat{y} - y)^2$

> 💡 **Additional Context (from assistant):** ReLU (Rectified Linear Unit) is $\text{ReLU}(z) = \max(0, z)$, with derivative $1$ if $z > 0$ and $0$ if $z \leq 0$. It's the most popular activation function in deep learning due to simplicity and no vanishing gradient for positive inputs.

### 9.2 Forward Pass (Numerical Example)

**Parameters:** $w^{(1)} = 2$, $b^{(1)} = -1$, $w^{(2)} = -3$, $b^{(2)} = 0$

**Input:** $x = 2$, **target:** $y = 4$

```
x = 2
↓
z(1) = w(1)·x + b(1) = 2·2 - 1 = 3
↓
h = ReLU(z(1)) = 3
↓
ŷ = w(2)·h + b(2) = -3·3 + 0 = -9
↓
L = ½(ŷ - y)² = ½(-9 - 4)² = ½·169 = 84.5
```

### 9.3 Backward Pass (Gradient Computation)

```
∂L/∂ŷ = ŷ - y = -9 - 4 = -13
↓
∂L/∂w(2) = (∂L/∂ŷ)·h = (-13)·3 = -39
∂L/∂b(2) = ∂L/∂ŷ = -13
↓
∂L/∂h = (∂L/∂ŷ)·w(2) = (-13)·(-3) = 39
↓
∂h/∂z(1) = 1  (because z(1) = 3 > 0)
↓
∂L/∂z(1) = (∂L/∂h)·(∂h/∂z(1)) = 39·1 = 39
↓
∂L/∂w(1) = (∂L/∂z(1))·x = 39·2 = 78
∂L/∂b(1) = ∂L/∂z(1) = 39
```

**Final Gradients:**

| Parameter | Gradient |
|-----------|----------|
| $\nabla_{w^{(1)}}$ | 78 |
| $\nabla_{b^{(1)}}$ | 39 |
| $\nabla_{w^{(2)}}$ | -39 |
| $\nabla_{b^{(2)}}$ | -13 |

> ⚠️ **Exam Tip:** Backpropagation is just the chain rule applied recursively from the output back to the inputs. Always go layer by layer, computing $\frac{\partial L}{\partial \text{layer output}}$ before $\frac{\partial L}{\partial \text{layer parameters}}$.

🧭 **Exam Priority: HIGH** — be able to do one full forward pass and one backward pass numerically.

**Additional Examples — Backprop:**

1. **Different activation (sigmoid):** If $h = \sigma(z^{(1)}) = \frac{1}{1+e^{-z^{(1)}}}$, then $\frac{\partial h}{\partial z^{(1)}} = h(1-h)$.

2. **Change input:** With $x = 1$ (same parameters):
   - Forward: $z^{(1)} = 1$, $h = 1$, $\hat{y} = -3$, $L = \frac{1}{2}(-3-4)^2 = 24.5$
   - Backward: $\frac{\partial L}{\partial \hat{y}} = -7$, $\frac{\partial L}{\partial w^{(2)}} = -7$, $\frac{\partial L}{\partial w^{(1)}} = (-7)(-3)(1)(1) = 21$

3. **With ReLU at negative pre-activation:** If $z^{(1)} = -1$, then $h = 0$, and **all gradients through $w^{(1)}$ and $b^{(1)}$ are 0** (dead neuron).

---

## 10. Further Reading and Applications

🧭 **Exam Priority: LOW-MEDIUM** — usually less central than core derivative/Jacobian/Hessian computations, but useful for theory/context questions.

### 10.1 Taylor Series in Machine Learning

The Taylor series has important applications in ML:
- **Extended Kalman Filter:** Uses first-order Taylor expansion to linearize nonlinear state-space models for online estimation.
- **Laplace Approximation:** Uses second-order Taylor expansion (requiring the Hessian) to approximate a posterior distribution $p(x)$ locally as a Gaussian around its mode.
- **Unscented Transform:** An alternative that does not require gradients.

### 10.2 Computing Expectations

Often in ML, we need:

$$\mathbb{E}_x[f(x)] = \int f(x) p(x) \, dx$$

If $p(x) = \mathcal{N}(\mu, \Sigma)$, a first-order Taylor expansion of $f$ around $\mu$ gives:

$$f(x) \approx f(\mu) + \nabla f(\mu)^\top (x - \mu)$$

This linearizes $f$, and for a Gaussian $p(x)$, the mean and covariance can then be computed exactly.

---

## 11. Exercises with Full Solutions

### Exercise 5.1: Derivative of $f(x) = \log(x^4)\sin(x^3)$

**Using the product rule:** $(uv)' = u'v + uv'$

- $u = \log(x^4) = 4\log(x)$, so $u' = \frac{4}{x}$
- $v = \sin(x^3)$, so $v' = 3x^2 \cos(x^3)$

**Step-by-step (simplified):**

1. Split the function into two factors: $u=\log(x^4)$ and $v=\sin(x^3)$.
2. Differentiate each factor: $u'=\frac{4}{x}$ and $v'=3x^2\cos(x^3)$.
3. Apply product rule: $f'=u'v+uv'$.
4. Substitute and simplify.

$$f'(x) = \frac{4}{x}\sin(x^3) + 4\log(x) \cdot 3x^2\cos(x^3)$$
$$= \frac{4\sin(x^3)}{x} + 12x^2 \log(x)\cos(x^3)$$

🧭 **Exam Priority: HIGH** — this type mixes product rule + chain rule and is very exam-like.

**Additional Examples:**

1. $f(x) = \log(x^2)\cos(x)$:
   - $f'(x) = \frac{2}{x}\cos(x) - \log(x^2)\sin(x)$

2. $f(x) = \ln(x)\cdot e^x$:
   - $f'(x) = \frac{e^x}{x} + e^x \ln(x) = e^x\left(\frac{1}{x} + \ln x\right)$

3. $f(x) = \log(x^3)\tan(x)$:
   - $f'(x) = \frac{3}{x}\tan(x) + 3\log(x)\sec^2(x)$

---

### Exercise 5.2: Derivative of Logistic Sigmoid $f(x) = \frac{1}{1 + e^{-x}}$

Using the quotient rule or chain rule:

**Step-by-step (simplified):**

1. Start from $f(x)=\frac{1}{1+e^{-x}}$.
2. Differentiate once:

$$f'(x) = \frac{e^{-x}}{(1 + e^{-x})^2}$$

3. Rewrite in terms of $f(x)$:

$$f'(x) = \frac{1}{1+e^{-x}} \cdot \frac{e^{-x}}{1+e^{-x}} = f(x) \cdot (1 - f(x))$$

> ⚠️ **Exam Tip:** $\sigma'(x) = \sigma(x)(1 - \sigma(x))$ is the most elegant and commonly used form. Memorize it — it appears everywhere in logistic regression and neural networks.

🧭 **Exam Priority: HIGH** — this identity is frequently used directly in exam solutions.

**Additional Examples:**

1. $f(x) = \frac{1}{1 + e^{-2x}}$ (scaled sigmoid):
   - $f'(x) = 2f(x)(1-f(x))$

2. $f(x) = \tanh(x) = \frac{e^x - e^{-x}}{e^x + e^{-x}}$:
   - $f'(x) = 1 - \tanh^2(x) = \text{sech}^2(x)$

3. $f(x) = \frac{1}{1 + e^{-(ax+b)}}$:
   - $f'(x) = a \cdot f(x)(1 - f(x))$

---

### Exercise 5.3: Derivative of Gaussian $f(x) = \exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)$

Using the chain rule with $u = -\frac{(x-\mu)^2}{2\sigma^2}$:

**Step-by-step (simplified):**

1. Define the inside function: $u = -\frac{(x-\mu)^2}{2\sigma^2}$.
2. Differentiate the outside function $e^u$: derivative is $e^u\cdot u'$.
3. Differentiate the inside function: $u'=-\frac{x-\mu}{\sigma^2}$.
4. Multiply outside and inside derivatives.

$$f'(x) = e^u \cdot u' = f(x) \cdot \left(-\frac{2(x-\mu)}{2\sigma^2}\right) = -\frac{x-\mu}{\sigma^2} f(x)$$

🧭 **Exam Priority: MEDIUM-HIGH** — common in Gaussian-model and gradient calculations.

> 💡 **Additional Context (from assistant):** This is the derivative of the Gaussian density (unnormalized). Notice it equals zero at $x = \mu$ (the peak), negative for $x > \mu$ (decreasing), and positive for $x < \mu$ (increasing).

**Additional Examples:**

1. $f(x) = e^{-x^2}$ (standard Gaussian kernel):
   - $f'(x) = -2xe^{-x^2}$

2. $f(x) = e^{-(x-3)^2/8}$ ($\mu=3, \sigma^2=4$):
   - $f'(x) = -\frac{x-3}{4} e^{-(x-3)^2/8}$

3. $f(x) = e^{-(x^2 + y^2)/2}$ (2D Gaussian):
   - $\frac{\partial f}{\partial x} = -x \cdot f(x,y)$, $\frac{\partial f}{\partial y} = -y \cdot f(x,y)$

---

### Exercise 5.4: Taylor Polynomials $T_n$ of $f(x) = \sin(x) + \cos(x)$ at $x_0 = 0$

Compute derivatives at 0:

| $n$ | $f^{(n)}(x)$ | $f^{(n)}(0)$ |
|-----|--------------|--------------|
| 0 | $\sin x + \cos x$ | 1 |
| 1 | $\cos x - \sin x$ | 1 |
| 2 | $-\sin x - \cos x$ | -1 |
| 3 | $-\cos x + \sin x$ | -1 |
| 4 | $\sin x + \cos x$ | 1 |
| 5 | $\cos x - \sin x$ | 1 |

$$T_0 = 1$$
$$T_1 = 1 + x$$
$$T_2 = 1 + x - \frac{x^2}{2}$$
$$T_3 = 1 + x - \frac{x^2}{2} - \frac{x^3}{6}$$
$$T_4 = 1 + x - \frac{x^2}{2} - \frac{x^3}{6} + \frac{x^4}{24}$$
$$T_5 = 1 + x - \frac{x^2}{2} - \frac{x^3}{6} + \frac{x^4}{24} + \frac{x^5}{120}$$

### Exercise 5.5: Jacobians

**5.5.a** $f_1(x) = \sin(x_1)\cos(x_2)$, $x \in \mathbb{R}^2$:

$$\frac{\partial f_1}{\partial x} = [\cos(x_1)\cos(x_2), -\sin(x_1)\sin(x_2)] \in \mathbb{R}^{1\times 2}$$

**5.5.b** $f_2(x, y) = x^\top y$, $x, y \in \mathbb{R}^n$ — result is scalar:

$$\frac{\partial f_2}{\partial x} = y^\top, \quad \frac{\partial f_2}{\partial y} = x^\top$$

**5.5.c** $f_3(x) = xx^\top$, $x \in \mathbb{R}^n$ — result is $n \times n$ matrix, Jacobian is a tensor $\in \mathbb{R}^{n \times n \times n}$:

$$\frac{\partial (xx^\top)_{ij}}{\partial x_k} = \delta_{ik}x_j + x_i \delta_{jk}$$

---

## 📝 Review Questions (30 Questions)

### Foundational Questions

**Q1. ⚠️ What is a partial derivative? How is it computed?**

🧭 **Exam Priority: HIGH**

A partial derivative $\frac{\partial f}{\partial x_i}$ measures the rate of change of $f$ with respect to $x_i$, keeping all other variables constant. Computed by treating all other variables as constants and differentiating normally.

*Example:* $f(x_1, x_2) = x_1^2 + x_1 x_2 \Rightarrow \frac{\partial f}{\partial x_1} = 2x_1 + x_2$

---

**Q2. What is the gradient $\nabla f$? What are its dimensions?**

🧭 **Exam Priority: HIGH**

For $f: \mathbb{R}^n \to \mathbb{R}$, $\nabla f \in \mathbb{R}^{1 \times n}$ is a row vector of all partial derivatives. It points in the direction of steepest ascent.

---

**Q3. ⚠️ What is the Jacobian? What are its dimensions?**

🧭 **Exam Priority: HIGH**

For $f: \mathbb{R}^n \to \mathbb{R}^m$, the Jacobian $J \in \mathbb{R}^{m \times n}$ has $(i,j)$-entry $\frac{\partial f_i}{\partial x_j}$. Dimensions: rows = number of outputs, columns = number of inputs.

---

**Q4. State the chain rule for $f: \mathbb{R}^n \to \mathbb{R}$, $\mathbf{x}: \mathbb{R} \to \mathbb{R}^n$.**

🧭 **Exam Priority: HIGH**

$$\frac{d(f \circ \mathbf{x})}{dt} = \sum_{i=1}^n \frac{\partial f}{\partial x_i} \frac{dx_i}{dt} = \nabla_x f \cdot \dot{\mathbf{x}}$$

---

**Q5. ⚠️ What is the Hessian? When is it symmetric?**

🧭 **Exam Priority: HIGH**

The Hessian $H = \nabla^2 f \in \mathbb{R}^{n \times n}$ collects all second partial derivatives. It is symmetric when $f$ is twice continuously differentiable (Schwarz's theorem: $\frac{\partial^2 f}{\partial x_i \partial x_j} = \frac{\partial^2 f}{\partial x_j \partial x_i}$).

---

**Q6. What does the Hessian tell us about a critical point?**

🧭 **Exam Priority: HIGH**

At a critical point $\nabla f = 0$:
- $H$ positive definite → local minimum
- $H$ negative definite → local maximum
- $H$ indefinite → saddle point

---

**Q7. Compute $\nabla f$ for $f(x_1, x_2) = 3x_1^2 - 2x_1 x_2 + x_2^3$.**

🧭 **Exam Priority: MEDIUM-HIGH**

$$\nabla f = [6x_1 - 2x_2,\ -2x_1 + 3x_2^2]$$

---

**Q8. Compute the Jacobian of $f: \mathbb{R}^2 \to \mathbb{R}^2$, $f(x_1, x_2) = (x_1^2 + x_2, x_1 - x_2^2)$.**

🧭 **Exam Priority: HIGH**

$$J = \begin{bmatrix} 2x_1 & 1 \\ 1 & -2x_2 \end{bmatrix}$$

---

**Q9. ⚠️ Derive the derivative of the logistic sigmoid $\sigma(x) = \frac{1}{1+e^{-x}}$.**

🧭 **Exam Priority: HIGH**

$$\sigma'(x) = \sigma(x)(1 - \sigma(x))$$

*Derivation:* $\sigma'(x) = \frac{e^{-x}}{(1+e^{-x})^2} = \frac{1}{1+e^{-x}} \cdot \frac{e^{-x}}{1+e^{-x}} = \sigma(x)(1-\sigma(x))$

---

**Q10. What is the derivative of the Gaussian $f(x) = e^{-(x-\mu)^2/(2\sigma^2)}$?**

🧭 **Exam Priority: MEDIUM-HIGH**

$$f'(x) = -\frac{x - \mu}{\sigma^2} f(x)$$

---

### Intermediate Questions

**Q11. ⚠️ State the multivariate Taylor series definition.**

🧭 **Exam Priority: MEDIUM-HIGH**

For smooth $f: \mathbb{R}^D \to \mathbb{R}$, with $\delta = x - x_0$:

$$f(x) = \sum_{k=0}^\infty \frac{D_x^k f(x_0)}{k!} \delta^k$$

---

**Q12. Write out explicitly the terms for $k = 0, 1, 2$ in the Taylor series.**

🧭 **Exam Priority: MEDIUM**

- $k=0$: $f(x_0)$
- $k=1$: $\nabla_x f(x_0) \cdot \delta$
- $k=2$: $\frac{1}{2}\delta^\top H(x_0) \delta$

---

**Q13. ⚠️ Compute the Taylor polynomial $T_2$ of $f(x,y) = e^{x+y}$ at $(0,0)$.**

🧭 **Exam Priority: MEDIUM-HIGH**

- $f(0,0) = 1$, $\nabla f = [1,1]$, $H = \begin{bmatrix}1&1\\1&1\end{bmatrix}$
- $T_2(x,y) = 1 + x + y + \frac{1}{2}(x^2 + 2xy + y^2)$

---

**Q14. What is a Taylor polynomial of degree $n$?**

🧭 **Exam Priority: MEDIUM**

It is $T_n(x) = \sum_{k=0}^n \frac{D_x^k f(x_0)}{k!} \delta^k$ — the first $n+1$ terms of the Taylor series.

---

**Q15. What is the outer product $\delta^2$ of a vector $\delta \in \mathbb{R}^D$?**

🧭 **Exam Priority: LOW**

$\delta^2 = \delta \otimes \delta = \delta\delta^\top \in \mathbb{R}^{D \times D}$, with $\delta^2[i,j] = \delta[i]\delta[j]$.

---

**Q16. ⚠️ Derive the gradient of the least-squares loss $L = \|y - \Phi\theta\|^2$.**

🧭 **Exam Priority: HIGH**

Let $e = y - \Phi\theta$. Then:
$$\nabla_\theta L = \frac{\partial L}{\partial e} \cdot \frac{\partial e}{\partial \theta} = 2e^\top \cdot (-\Phi) = -2(y - \Phi\theta)^\top\Phi = 2\theta^\top\Phi^\top\Phi - 2y^\top\Phi$$

Setting to zero gives the normal equations: $\Phi^\top\Phi\hat\theta = \Phi^\top y$.

---

**Q17. Compute the Hessian of $f(x,y) = x^2 + 2xy + y^3$.**

🧭 **Exam Priority: MEDIUM-HIGH**

$$H = \begin{bmatrix} 2 & 2 \\ 2 & 6y \end{bmatrix}$$

---

**Q18. What is the chain rule for $f: \mathbb{R}^n \to \mathbb{R}$, $\mathbf{x}: \mathbb{R}^m \to \mathbb{R}^n$?**

🧭 **Exam Priority: HIGH**

$$\frac{\partial(f \circ \mathbf{x})}{\partial \mathbf{s}} = \underbrace{\nabla_\mathbf{x} f}_{1 \times n} \cdot \underbrace{J_\mathbf{x}}_{n \times m} \in \mathbb{R}^{1 \times m}$$

---

**Q19. When does the Taylor series of a polynomial give an exact representation?**

🧭 **Exam Priority: MEDIUM**

When the Taylor series is expanded to the same degree as the polynomial. A degree-$d$ polynomial's Taylor series is exact at order $d$ and all higher terms vanish.

---

**Q20. ⚠️ Apply the chain rule: $f(x_1,x_2) = x_1 + 2x_2$, $\mathbf{x}(t) = (\sin t, \cos t)$.**

🧭 **Exam Priority: HIGH**

$$\nabla f = [1, 2], \quad \dot{\mathbf{x}} = [\cos t, -\sin t]^\top$$
$$\frac{d(f \circ \mathbf{x})}{dt} = \cos t + 2(-\sin t) = \cos t - 2\sin t$$

---

### Advanced Questions

**Q21. ⚠️ Describe backpropagation. What mathematical principle underlies it?**

🧭 **Exam Priority: HIGH**

Backpropagation computes gradients of a loss function $L$ with respect to all parameters in a neural network by applying the **chain rule** layer by layer from output to input. At each layer, we compute $\frac{\partial L}{\partial \text{layer output}}$ and multiply by the local Jacobian to propagate backward.

---

**Q22. ⚠️ Carry out the full forward and backward pass for $w^{(1)}=1, b^{(1)}=0, w^{(2)}=2, b^{(2)}=1$, input $x=3$, target $y=10$.**

🧭 **Exam Priority: HIGH**

*Forward:*
- $z^{(1)} = 1 \cdot 3 + 0 = 3$
- $h = \text{ReLU}(3) = 3$
- $\hat{y} = 2 \cdot 3 + 1 = 7$
- $L = \frac{1}{2}(7-10)^2 = 4.5$

*Backward:*
- $\frac{\partial L}{\partial \hat{y}} = 7 - 10 = -3$
- $\frac{\partial L}{\partial w^{(2)}} = -3 \cdot 3 = -9$, $\frac{\partial L}{\partial b^{(2)}} = -3$
- $\frac{\partial L}{\partial h} = -3 \cdot 2 = -6$
- $\frac{\partial h}{\partial z^{(1)}} = 1$ (since $z^{(1)} = 3 > 0$)
- $\frac{\partial L}{\partial w^{(1)}} = -6 \cdot 3 = -18$, $\frac{\partial L}{\partial b^{(1)}} = -6$

---

**Q23. What happens to gradients when a ReLU neuron has a negative pre-activation?**

🧭 **Exam Priority: MEDIUM-HIGH**

The gradient is **zero** (the neuron is "dead"). No gradient flows back through that neuron, meaning parameters feeding it will not update. This is the "dying ReLU" problem.

---

**Q24. What is the Laplace approximation and how does it use the Hessian?**

🧭 **Exam Priority: LOW-MEDIUM**

The Laplace approximation approximates a distribution $p(x)$ as a Gaussian centered at its mode $x^*$. Using a second-order Taylor expansion of $\log p(x)$:

$$\log p(x) \approx \log p(x^*) - \frac{1}{2}(x - x^*)^\top H(x^*)(x - x^*)$$

This gives approximation $p(x) \approx \mathcal{N}(x^*, H^{-1}(x^*))$.

---

**Q25. How is the Taylor series used in the Extended Kalman Filter?**

🧭 **Exam Priority: LOW-MEDIUM**

The EKF uses a **first-order Taylor expansion** of the nonlinear measurement/transition function $f$ around the current state estimate $\mu$:

$$f(x) \approx f(\mu) + J_f(\mu)(x - \mu)$$

This linearizes the function, allowing the standard Kalman filter update equations to be applied.

---

**Q26. ⚠️ Compute the full Taylor series expansion of $f(x,y) = x^2 + 2xy + y^3$ at $(1,2)$.**

🧭 **Exam Priority: HIGH**

$$f(x,y) = 13 + 6(x-1) + 14(y-2) + (x-1)^2 + 2(x-1)(y-2) + 6(y-2)^2 + (y-2)^3$$

(Full derivation in Section 8.6 above.)

---

**Q27. Explain what $D_x^k f(x_0) \delta^k$ means for $k = 3$.**

🧭 **Exam Priority: LOW-MEDIUM**

It is:
$$\sum_{i=1}^{D}\sum_{j=1}^{D}\sum_{k=1}^{D} D^3 f(x_0)[i,j,k] \cdot \delta[i]\delta[j]\delta[k]$$

A sum of all cubic monomials in $\delta$, weighted by the third-order tensor $D^3 f$.

---

**Q28. ⚠️ Compute $f'(x) = \frac{d}{dx}[\log(x^4)\sin(x^3)]$.**

🧭 **Exam Priority: HIGH**

$$f'(x) = \frac{4\sin(x^3)}{x} + 12x^2\log(x)\cos(x^3)$$

(Full derivation in Exercise 5.1.)

---

**Q29. What are the dimensions of the Hessian for $f: \mathbb{R}^n \to \mathbb{R}^m$?**

🧭 **Exam Priority: MEDIUM**

For a vector-valued function $f: \mathbb{R}^n \to \mathbb{R}^m$, the Hessian is an $(m \times n \times n)$ **tensor** — one $n \times n$ Hessian matrix per output dimension.

---

**Q30. ⚠️ Derive the normal equations from the least-squares gradient.**

🧭 **Exam Priority: HIGH**

From $\nabla_\theta L = -2\Phi^\top(y - \Phi\theta) = 0$:

$$\Phi^\top(y - \Phi\theta) = 0$$
$$\Phi^\top y = \Phi^\top \Phi \theta$$
$$\hat\theta = (\Phi^\top \Phi)^{-1} \Phi^\top y \quad \text{(if } \Phi^\top\Phi \text{ is invertible)}$$

This is the **closed-form ordinary least-squares solution**.

---

*Notes compiled from MML Chapter 5 slides — Statistical and Mathematical Methods for Artificial Intelligence, University of Bologna. Based on: Deisenroth, Faisal, Ong — "Mathematics for Machine Learning" (Cambridge University Press, 2020).*
