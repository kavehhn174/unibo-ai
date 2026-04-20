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

📝 **Important for the exam:** Partial derivatives are a basic skill that often appears in short questions, so practice them until they are automatic.

**Example (step by step):** Given $f(x_1, x_2) = x_1^2 + x_1 x_2$:

1. Differentiate with respect to $x_1$ and keep $x_2$ fixed:

$$\frac{\partial f}{\partial x_1} = 2x_1 + x_2$$

2. Differentiate with respect to $x_2$ and keep $x_1$ fixed:

$$\frac{\partial f}{\partial x_2} = x_1$$

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

📝 **Important for the exam:** These examples cover the most common algebra patterns: powers, products, exponentials, and logarithms.

---

## 2. Gradients

For $f: \mathbb{R}^n \to \mathbb{R}$, the **gradient** is a row vector:

$$\nabla_x f = \left[ \frac{\partial f}{\partial x_1}, \frac{\partial f}{\partial x_2}, \ldots, \frac{\partial f}{\partial x_n} \right] \in \mathbb{R}^{1 \times n}$$

> ⚠️ **Exam Tip:** The gradient points in the direction of steepest ascent of $f$. This is crucial for optimization (gradient descent moves opposite to the gradient).

📝 **Important for the exam:** Gradients are often used to find stationary points and to explain gradient descent, so they are a core exam topic.

> 💡 **Additional Context (from assistant):** Think of the gradient like a "slope compass" in multiple dimensions. In 2D, it tells you which direction goes uphill the fastest.

**Example (step by step):** $f(x_1, x_2) = x_1^2 + x_1 x_2$

1. Compute the partial derivatives:

$$\frac{\partial f}{\partial x_1} = 2x_1 + x_2, \quad \frac{\partial f}{\partial x_2} = x_1$$

2. Put the derivatives together as a gradient row vector:

$$\nabla f = \left[ 2x_1 + x_2, \quad x_1 \right]$$

3. Evaluate at the point $(1, 2)$:

$$\nabla f(1,2) = [4, 1]$$

**Additional Examples:**

1. $f(x_1, x_2) = 3x_1^2 - x_2^3$:
   - $\nabla f = [6x_1, -3x_2^2]$
   - At $(1, 1)$: $\nabla f = [6, -3]$

2. $f(x_1, x_2) = e^{x_1 + x_2}$:
   - $\nabla f = [e^{x_1+x_2}, e^{x_1+x_2}]$
   - At $(0,0)$: $\nabla f = [1, 1]$

3. $f(x_1, x_2, x_3) = x_1 x_2 + x_2 x_3 + x_3 x_1$:
   - $\nabla f = [x_2 + x_3, x_1 + x_3, x_2 + x_1]$

📝 **Important for the exam:** Many exam questions ask you to compute a gradient and then evaluate it at a point. Practice both parts together.

---

## 3. Chain Rule

### 3.1 Univariate Chain Rule

For composed functions $f \circ \mathbf{x}$ where $f: \mathbb{R}^n \to \mathbb{R}$ and $\mathbf{x}: \mathbb{R} \to \mathbb{R}^n$:

$$\frac{d(f \circ \mathbf{x})}{dt} = \frac{\partial f}{\partial x_1} \frac{dx_1}{dt} + \frac{\partial f}{\partial x_2} \frac{dx_2}{dt} = \nabla_x f \cdot \dot{\mathbf{x}}(t)$$

> ⚠️ **Exam Tip:** The chain rule is the backbone of backpropagation in neural networks. Know this cold.

📝 **Important for the exam:** Chain-rule problems usually hide a composition inside the formula, so first identify the outer function and the inner function.

**Example (step by step):** $f(x_1, x_2) = x_1^2 + 2x_2$, with $\mathbf{x}(t) = (\sin(t), \cos(t))$:

1. Differentiate the outer function with respect to each variable:

$$\frac{\partial f}{\partial x_1} = 2x_1, \quad \frac{\partial f}{\partial x_2} = 2$$

2. Differentiate the inner functions:

$$\frac{dx_1}{dt} = \cos(t), \quad \frac{dx_2}{dt} = -\sin(t)$$

3. Combine the two parts using the chain rule:

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

📝 **Important for the exam:** Once you know the pattern, the same chain-rule idea works even when the functions look different.

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

📝 **Important for the exam:** The Jacobian is a very common definition question. Memorize that rows correspond to outputs and columns correspond to inputs.

**Example (step by step):** $f: \mathbb{R}^2 \to \mathbb{R}^3$ with

$$f(x_1, x_2) = \begin{bmatrix} 2x_2 + x_1 \\ x_2 \\ x_1 \end{bmatrix}$$

1. Differentiate each output with respect to $x_1$ and $x_2$.

2. Put the derivatives into rows.

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

📝 **Important for the exam:** The polar-coordinate Jacobian is a classic exam point because it appears in change-of-variable integrals.

---

## 5. Gradients of Vector-Valued Functions

For the general chain rule with $f: \mathbb{R}^n \to \mathbb{R}^m$ and $g: \mathbb{R}^p \to \mathbb{R}^n$:

$$\frac{\partial(f \circ g)}{\partial \mathbf{x}} = \underbrace{\frac{\partial f}{\partial g}}_{m \times n} \cdot \underbrace{\frac{\partial g}{\partial \mathbf{x}}}_{n \times p} \in \mathbb{R}^{m \times p}$$

> ⚠️ **Exam Tip:** Always check matrix dimensions match when applying the chain rule in composed functions. The middle dimension must cancel.

📝 **Important for the exam:** This section is often tested with dimension checks, so it is worth memorizing the multiplication order.

📝 **Important for the exam:** Dimension checking is a fast way to catch mistakes before you lose points.

---

## 6. Least-Squares Loss and Gradient Derivation

This section covers a classic machine learning application of the Jacobian.

**Setup:** Given $\Phi \in \mathbb{R}^{N \times D}$ (feature matrix), $y \in \mathbb{R}^N$ (targets), $\theta \in \mathbb{R}^D$ (parameters):

- Prediction: $g(\theta) = \Phi\theta \in \mathbb{R}^N$
- Error: $e(\theta) = y - g(\theta) = y - \Phi\theta \in \mathbb{R}^N$
- Loss (squared): $L(e) = \|e\|^2 = e^\top e$

**Gradient of the Least-Squares Loss:**

Using the chain rule $\nabla(L \circ e)(\theta) = \frac{\partial L}{\partial e} \cdot \frac{\partial e}{\partial \theta}$:

1. Start with the error vector:

$$e(\theta) = y - \Phi\theta$$

2. Write the squared loss as an inner product:

$$L(e) = e^\top e$$

3. Differentiate the loss with respect to the error, and the error with respect to the parameters:

$$\frac{\partial L}{\partial e} = 2e^\top \in \mathbb{R}^{1 \times N}$$

$$\frac{\partial e}{\partial \theta} = -\Phi \in \mathbb{R}^{N \times D}$$

4. Multiply the two pieces:

$$\nabla_\theta L = \frac{\partial L}{\partial e} \cdot \frac{\partial e}{\partial \theta} = 2e^\top \cdot (-\Phi) = -2e^\top \Phi = -2(y - \Phi\theta)^\top \Phi$$

$$= -2y^\top \Phi + 2\theta^\top \Phi^\top \Phi$$

> ⚠️ **Exam Tip:** This derivation is often asked in exams. The final gradient $\nabla_\theta L = 2(\Phi^\top \Phi \theta - \Phi^\top y)$ leads to the **normal equations** $\Phi^\top \Phi \theta = \Phi^\top y$ when set to zero.

> 💡 **Additional Context (from assistant):** Setting $\nabla_\theta L = 0$ gives the closed-form ordinary least-squares solution $\hat{\theta} = (\Phi^\top \Phi)^{-1} \Phi^\top y$, assuming $\Phi^\top \Phi$ is invertible.

📝 **Important for the exam:** This derivation is a high-value exam result because it connects calculus, linear algebra, and least squares in one place.

📝 **Important for the exam:** Be ready to rewrite the final gradient in either compact matrix form or expanded transposed form, because the professor may use either notation.
