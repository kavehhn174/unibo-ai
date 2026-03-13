<img src="https://r2cdn.perplexity.ai/pplx-full-logo-primary-dark%402x.png" style="height:64px;margin-right:32px"/>

## What is in the picture?

From the photo there are three main parts (all within the topic “Functions and encodings”). Each part connects directly to sections in your slides.[^1][^2]

1. Basic definitions
    - Cartesian product $X\times Y$
    - Relation on $X,Y$
    - Function $f:X\to Y$
2. About cardinalities
    - Size of $S^n$ and $|Y^X|$
    - Example: how many functions from $n$ bits to $m$ bits?
3. Exercises
    - Exercise A: relate pairs of functions using $O,\Omega,\Theta$
    - Exercise B: design encodings as binary strings for

4) rational numbers $\mathbb Q$
5) disjoint union $\mathbb N\cup\{0,1\}^*$
6) directed graphs $(V,E)$

Below, each exercise is rewritten in clean typed form, then solved step by step with simple explanations and examples. At the end there are extra questions with full worked solutions.

***

## 1. Basic notions (from slides and photo)

### 1.1 Cartesian product

Definition (simple):
Given two sets $X$ and $Y$, the cartesian product $X\times Y$ is the set of all ordered pairs where the first component comes from $X$ and the second from $Y$.[^2]

Formally:

$$
X\times Y = \{(x,y)\mid x\in X,\ y\in Y\}.
$$

Example:

- Let $X=\{1,2\}$, $Y=\{a,b\}$.
- Then $X\times Y = \{(1,a),(1,b),(2,a),(2,b)\}$.

Intuition:
Think of a table: rows labeled by elements of $X$, columns by elements of $Y$. Every cell is one pair $(x,y)$.

***

### 1.2 Relation

Definition (simple):
A relation between $X$ and $Y$ is just a *set of pairs* taken from $X\times Y$.[^2]

- So any subset $R\subseteq X\times Y$ is a relation.
- Relations can be arbitrary: some $x$ may not appear at all; some may be paired with many $y$’s.

Example:

- $X=\{1,2\}$, $Y=\{a,b\}$.
- Relation $R = \{(1,a),(2,a)\}$.
- Here 1 is related only to a; 2 is related only to a; b is never used.

***

### 1.3 Function

Formal slide definition: a function $f$ from $X$ to $Y$ is a relation on $X$ and $Y$ such that for every $x\in X$ there exists *exactly one* $y\in Y$ with $(x,y)\in f$.[^2]

Simplified meaning:

- Domain $X$: the set of all possible inputs.
- Codomain $Y$: the set of allowed outputs.
- For each input $x$ you must have:
    - at least one output (no missing values)
    - at most one output (no ambiguity).

So “exactly one” means:

- you never have *zero* outputs for an $x$ (totality)
- you never have *two different* outputs for the same $x$ (deterministic).

Example:

- $X=\{1,2,3\}$, $Y=\{a,b\}$.
- $f = \{(1,a),(2,b),(3,a)\}$ is a function: every input 1,2,3 appears once.
- $g = \{(1,a),(1,b),(2,a)\}$ is *not* a function: 1 is mapped to both a and b.

Graph picture (shape):
Think of arrows:

- Draw all elements of $X$ in a row on the left.
- Draw all elements of $Y$ on the right.
- For a function, each left dot has exactly one arrow to a right dot.

***

## 2. Cardinalities of function spaces

### 2.1 Strings of length $n$

Claim: if a finite set $S$ has $|S|$ elements, then the set of strings of length $n$ over $S$, written $S^n$, has size $|S^n| = |S|^n$.[^1][^2]

Intuition:
To build a string of length $n$:

- You have $|S|$ choices for position 1,
- $|S|$ choices for position 2,
- …
- $|S|$ choices for position $n$.

Total number of strings is the product of the choices:

$$
|S^n| = \underbrace{|S|\cdot |S|\cdots |S|}_{n\text{ times}} = |S|^n.
$$

Example:

- $S=\{0,1\}$, $|S|=2$.
- Strings of length 3: $000,001,010,011,100,101,110,111$. There are $2^3=8$ strings.

The handwritten page proves this formally by induction on $n$, but the idea above is the same.[^1]

***

### 2.2 Number of functions $Y^X$

Notation: $Y^X$ means “the set of all functions from $X$ to $Y$”.[^1][^2]

Claim:
If $X$ and $Y$ are finite, then the number of functions from $X$ to $Y$ is

$$
|Y^X| = |Y|^{|X|}.
$$

Reason:
Think of choosing the output for each input in $X$.

- For each element $x\in X$, the function must pick one element of $Y$.
- For every $x$ there are $|Y|$ possible choices.
- Choices for different $x$’s are independent.
- There are $|X|$ inputs, so:

$$
|Y^X| = \underbrace{|Y|\cdot|Y|\cdots|Y|}_{|X|\text{ times}} = |Y|^{|X|}.
$$

Example:

- $X=\{1,2\}$ (so $|X|=2$), $Y=\{a,b,c\}$ (so $|Y|=3$).
- A function $f:X\to Y$ is defined by the pair of values $(f(1),f(2))$.
- There are 3 choices for $f(1)$ and 3 for $f(2)$, total $3^2=9$ functions.

Shape view:
Picture $X$ as a column of $|X|$ rows. For each row (input) you choose one element of $Y$. The full column of choices is one function. [^1]

***

### 2.3 Example: functions from $n$ bits to $m$ bits

Question on the sheet:

“How many functions from $n$ bits to $m$ bits can one build?”[^1]

- “n bits” means all strings of length $n$ over $\{0,1\}$: that set is $\{0,1\}^n$.
    - Size of the domain: $|\{0,1\}^n| = 2^n$.
- “m bits” means all strings of length $m$ over $\{0,1\}$: set $\{0,1\}^m$.
    - Size of the codomain: $|\{0,1\}^m| = 2^m$.

By the formula above, number of functions is

$$
|\{0,1\}^m{}^{\{0,1\}^n}| 
= (2^m)^{2^n}
= 2^{m\cdot 2^n}.
$$

So there are $2^{m\cdot 2^n}$ different functions.[^1]

Example:

- $n=1$, $m=1$: domain $\{0,1\}$, codomain $\{0,1\}$.
    - There are $2^{1\cdot 2^1}=2^2=4$ possible functions (the usual four truth tables of a unary boolean function).

***

## 3. Exercise: compare pairs of functions using $O,\Omega,\Theta$

### 3.1 Asymptotic notation reminder

From the slides: for functions $f,g:\mathbb N\to\mathbb N$.[^2]

- $f(n)$ is $O(g(n))$
if there exist a constant $c>0$ such that
$f(n) \le c\cdot g(n)$ for all sufficiently large $n$.
- $f(n)$ is $\Omega(g(n))$
if there exists $c>0$ with
$f(n)\ge c\cdot g(n)$ for all sufficiently large $n$.
- $f(n)$ is $\Theta(g(n))$
if it is both $O(g(n))$ and $\Omega(g(n))$.

Practical trick:
Often we compare the limit of the ratio:

$$
L = \lim_{n\to\infty}\frac{f(n)}{g(n)}.
$$

- If $0<L<\infty$: $f(n)$ and $g(n)$ grow at the same rate, so $f\in\Theta(g)$.
- If $L=0$: $f\in O(g)$ but not $\Omega(g)$.
- If $L=+\infty$: $f\in \Omega(g)$ but not $O(g)$.[^2]

***

### 3.2 The specific exercise (typed)

The handwritten “Exercise: relate the following pairs of functions by way of the asymptotic operators” uses two pairs:[^1]

1. Pair 1

$$
f_1(n) = (\log n)^2,\quad g_1(n) = n.
$$
2. Pair 2

$$
f_2(n) = 10\,n\log(\log n),\quad g_2(n) = \frac{1}{100}\,n\log n.
$$

Goal: for each pair, say whether

- $f\in O(g)$?
- $f\in \Omega(g)$?
- $f\in \Theta(g)$?

The solution on the sheet uses limits and then states these relations.[^1]

***

### 3.3 Pair 1: $f_1(n) = (\log n)^2$, $g_1(n) = n$

Step 1: consider the ratio

$$
\frac{f_1(n)}{g_1(n)} = \frac{(\log n)^2}{n}.
$$

Intuition:

- $\log n$ grows very slowly.
- $n$ grows fast.
- A polynomial in $\log n$ is still much smaller than any positive power of $n$.

Formal (using known limit):

$$
\lim_{n\to\infty}\frac{(\log n)^k}{n^c} = 0
$$

for any fixed $k>0,c>0$. In our case $k=2,c=1$. So

$$
\lim_{n\to\infty}\frac{(\log n)^2}{n} = 0.
$$

Interpretation:

- Because the limit is 0, $f_1$ is strictly smaller order than $g_1$.
- Therefore $f_1\in O(g_1)$.
- Also $f_1\notin \Omega(g_1)$ and $f_1\notin\Theta(g_1)$.

So:

- $f_1\in O(g_1)$
- $f_1\notin \Omega(g_1)$
- $f_1\notin \Theta(g_1)$.

***

### 3.4 Pair 2: $f_2(n) = 10n\log(\log n)$, $g_2(n) = \frac{1}{100}n\log n$

Step 1: look at the ratio

$$
\frac{f_2(n)}{g_2(n)} 
= \frac{10n\log(\log n)}{\frac{1}{100}n\log n}
= 1000\,\frac{\log(\log n)}{\log n}.
$$

Step 2: analyze $\frac{\log(\log n)}{\log n}$.

- $\log(\log n)$ grows slower than $\log n$.
- As $n\to\infty$, the ratio goes to 0.

So

$$
\lim_{n\to\infty}\frac{f_2(n)}{g_2(n)} 
= 1000\cdot 0
=0.
$$

Therefore $f_2$ grows asymptotically smaller than $g_2$.[^2]

Conclusions:

- $f_2\in O(g_2)$.
- $f_2\notin \Omega(g_2)$.
- $f_2\notin \Theta(g_2)$.

The handwritten solution also notes $g_2\in \Omega(f_2)$ etc.[^1]

***

## 4. Exercise: encodings into binary strings

Now the second big exercise in the photo about encodings. It asks:

> “We would like to define encodings (representations into binary strings) of the following sets of objects.”[^2][^1]

The objects:

A. Rational numbers $\mathbb Q$.
B. The disjoint union $\mathbb N \cup \{0,1\}^*$ of natural numbers and binary strings.
C. Directed graphs $(V,E)$ where $V$ is a finite set of nodes and $E\subseteq V\times V$ is the set of arcs.

The idea matches your slides about “Representing objects as strings”.[^2]

***

### 4.1 Part A – Encoding rational numbers $\mathbb Q$

Goal: map every rational number (fraction) to a unique binary string.

Key observation (from solution):

- Any rational number $q$ can be expressed as $\frac{z}{n}$ where
    - $z\in\mathbb Z$ is an integer (can be negative),
    - $n\in\mathbb N$, $n>0$,
    - fraction is in *lowest terms* (no common non-trivial factor between $z$ and $n$).[^1]

So each rational corresponds uniquely to such a pair $(z,n)$.

Step-by-step encoding:

1. Reduce the fraction to lowest terms.
    - Example: $-4/6$ becomes $-2/3$.
2. Encode the numerator $z$:
    - Separate its *sign* and its *absolute value*.
    - Example: $-2$: sign “1” meaning negative, absolute value 2.
3. Encode each integer (absolute value of numerator and denominator) in binary, as in slide examples.[^2]
4. Put a separator symbol between numerator and denominator, for example “\#”.

So an encoding looks like:

$$
\text{enc}\left(\frac{z}{n}\right) = \text{enc}(z)\#\text{enc}(n).
$$

The handwritten example:

For $-2/3$ the solution writes something like

- encode $-2$ as “1 10” (1 for sign-, then “10” is 2 in binary),
- encode 3 as “11”,
- get the final string: `110#11` which then itself is turned into pure binary (using encoding of \# too).[^1][^2]

The exact bit pattern does not matter for the course, as long as:

- every rational has *some* encoding,
- the encoding is reversible (from the string you can uniquely recover the rational).

***

### 4.2 Part B – Encoding $\mathbb N\cup\{0,1\}^*$

This is a *disjoint union*: elements are either

- a natural number $n\in\mathbb N$, or
- a binary string $x\in\{0,1\}^*$.[^2][^1]

We must encode both kinds in such a way that it is clear which kind it is.

Standard trick: use a *marker* bit on the front.

Design from the solution:

- Use marker 0 for numbers, 1 for strings.[^1]

Encoding rules:

1. If the element is a natural number $n$:
    - Encode $n$ in binary in some fixed way (e.g., usual binary without leading zeros).
    - Prepend marker 0.
    - Example: number 5 is “101”; encoding is `0 101`.
2. If the element is a binary string $x$:
    - Just prepend marker 1 to $x$.
    - Example: string `0110`; encoding is `1 0110`.

Because numbers never start with the special separating marker and because we know the first bit (0 or 1) tells the type, the code is uniquely decodable.

This matches the idea in the handwritten notes where they talk about using labels 0 and 1 as markers.[^1]

***

### 4.3 Part C – Encoding finite directed graphs

A directed graph is given as $(V,E)$:

- $V$ is a finite set of nodes (often we may assume $V=\{1,\dots,k\}$).
- $E\subseteq V\times V$ is the set of directed edges (arcs).[^2][^1]

We need an encoding as binary strings.

One common encoding (consistent with your notes):

1. Assume the nodes are numbered $1,\dots,k$.
2. Consider every possible ordered pair $(i,j)$ with $1\le i,j\le k$. There are $k^2$ such pairs.
3. For each pair $(i,j)$, include one bit in the encoding:
    - bit 1 if there is an edge $i\to j$ (i.e., $(i,j)\in E$),
    - bit 0 otherwise.[^2]

So the encoding is:

- first encode $k$ (number of nodes) in binary,
- then a block of $k^2$ bits describing the adjacency matrix.

Example: small graph with three nodes and edges $1\to 2$, $2\to 3$; the matrix is

$$
\begin{array}{c|ccc}
  & 1 & 2 & 3\\\hline
1 & 0 & 1 & 0\\
2 & 0 & 0 & 1\\
3 & 0 & 0 & 0
\end{array}
$$

So the edge bits are `010001000`.
If $k=3$ is encoded as `11`, full encoding might be `11 010001000`.

On your sheet, two tiny graphs are drawn as examples of objects we want to encode.[^1]

***

## 5. Extra practice questions with full solutions

Here are some extra problems that match your exam topics (functions, cardinality, asymptotics, encodings), plus step-by-step solutions.

***

### Extra 1 – Number of functions, variation

**Question:**
Let $X$ be a set of size 4 and $Y$ a set of size 10.

1. How many functions $f:X\to Y$ are there?
2. How many *injective* functions $f:X\to Y$ (all inputs get different outputs)?

**Solution:**

1. All functions:
    - Use the formula $|Y^X| = |Y|^{|X|}$.
    - Here $|X|=4$, $|Y|=10$.
    - So number of functions is

$$
10^4 = 10\cdot10\cdot10\cdot10 = 10000.
$$
2. Injective functions:
    - For injective functions, different inputs must map to different outputs.
    - Count them step by step:
        - First element of $X$: 10 choices in $Y$.
        - Second element: must be different, so 9 choices.
        - Third: 8 choices.
        - Fourth: 7 choices.
    - Total injective functions:

$$
10\cdot 9\cdot 8\cdot 7.
$$
    - Compute:
$10\cdot9=90$,
$90\cdot8=720$,
$720\cdot7=5040$.

So there are 5040 injective functions.

***

### Extra 2 – Asymptotic comparison

**Question:**
Compare $f(n) = n^2 + 3n$ and $g(n) = 10n^2$. Decide whether $f$ is $O(g)$, $\Omega(g)$, and $\Theta(g)$.

**Solution:**

Compute the ratio:

$$
\frac{f(n)}{g(n)} = \frac{n^2 + 3n}{10n^2}
= \frac{1}{10} + \frac{3}{10n}.
$$

Now take the limit as $n\to\infty$:

$$
\lim_{n\to\infty}\left(\frac{1}{10} + \frac{3}{10n}\right) 
= \frac{1}{10} + 0 
= \frac{1}{10}.
$$

This is a positive finite constant.

Interpretation:

- $f$ and $g$ grow at the same rate.
- So $f\in\Theta(g)$.
- That also means $f\in O(g)$ and $f\in \Omega(g)$.

***

### Extra 3 – Encoding pairs of natural numbers

**Question:**
Give a simple encoding of pairs of natural numbers $(m,n)\in\mathbb N\times\mathbb N$ as binary strings. Then encode the pair $(3,5)$ using your scheme.

**Solution:**

One standard scheme (similar to your slides on encoding pairs):[^2]

1. Encode each natural number in binary.
2. Separate them with a special marker symbol `#`.
3. Finally, if needed, turn `#` itself into a binary pattern (e.g., `11` if numbers never use that code), but for reasoning we can leave the `#` explicit.

So the encoding function is:

$$
\text{enc}(m,n) = \text{bin}(m)\#\text{bin}(n).
$$

Example for $(3,5)$:

- $\text{bin}(3) = 11$.
- $\text{bin}(5) = 101$.
- So the encoded pair is the string `11#101`.

If we also encode `#` as, say, `00`, then the final binary-only string is `11 00 101`.

The important part: from `11#101` we can uniquely get back the pair (3,5).

***

### Extra 4 – Deciding if a function is well-defined

**Question:**
Let $X = \{a,b,c\}$ and $Y = \{0,1\}$. For each relation below, say whether it is a function from $X$ to $Y$, and if yes, whether it is injective or surjective.

1. $R_1 = \{(a,0),(b,0),(c,0)\}$.
2. $R_2 = \{(a,0),(b,1)\}$.
3. $R_3 = \{(a,0),(a,1),(b,0),(c,1)\}$.

**Solution:**

Recall: a function must have *exactly one* pair for each input $x\in X$.[^2]

1. $R_1$:
    - a appears once, b once, c once.
    - Every element of $X$ appears exactly once: so $R_1$ is a function.
    - Injective? No, because $f(a)=f(b)=f(c)=0$, not all different.
    - Surjective? No: output 1 is never used.
2. $R_2$:
    - a and b appear once, but c does not appear at all.
    - Not every input has an output, so $R_2$ is *not* a function.
3. $R_3$:
    - a appears twice: $(a,0)$ and $(a,1)$.
    - That violates “at most one output per input”, so $R_3$ is *not* a function.

***

### Extra 5 – Encoding small graphs

**Question:**
Consider a graph $G=(V,E)$ with $V=\{1,2\}$ and edges $E=\{(1,1),(1,2)\}$. Encode it using the adjacency-matrix method: first encode $|V|$, then the 4 bits for pairs $(1,1),(1,2),(2,1),(2,2)$ in this fixed order.

**Solution:**

1. Number of nodes: $|V|=2$. Encode as `10` (binary of 2).
2. For the ordered pairs:
    - $(1,1)$: there is an edge → bit 1.
    - $(1,2)$: there is an edge → bit 1.
    - $(2,1)$: no edge → bit 0.
    - $(2,2)$: no edge → bit 0.
3. Edge bits: `1100`.
4. Full encoding: `10 1100`.

From `10 1100` you can reconstruct that there are 2 nodes and the edge set above.

***

This covers the topics present in your solved sheet (functions, counting, asymptotics, encodings) and ties them to the official slides.[^1][^2]

To focus your practice: which of these four topics do you currently find most confusing—functions vs relations, counting functions, $O/\Omega/\Theta$, or encodings—and what part about it feels unclear?

<div align="center">⁂</div>

[^1]: ICC-23022026.jpg

[^2]: preliminaries2-2.pdf

