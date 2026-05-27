# A Common Language: First Order Logic
### *Module 2 — Fundamentals of Knowledge Representation and Reasoning*
*Based on the course by Prof. Ing. Federico Chesani, DISI — Department of Informatics, Science and Engineering*

---

## Why Do We Need a Special Language?

Think about how you are reading this sentence right now. You are using natural language — English — to absorb ideas. Natural language is beautifully expressive; it can be poetic, persuasive, and nuanced. But that very richness is also its greatest flaw: **natural language is deeply ambiguous**.

Consider the phrase: *"I saw the man with the telescope."* Did you use the telescope to see the man? Or did you see a man who was carrying a telescope? Both readings are grammatically valid. In casual conversation, this kind of ambiguity is usually harmless — context clears things up. But when we are trying to formally represent *knowledge* and build systems that *reason automatically*, ambiguity is a serious problem. A machine does not have common sense to fill in gaps; it needs precise, unambiguous instructions.

> **Author's Note:** This is not a hypothetical problem. Early expert systems and AI programs suffered greatly from informal, ambiguous knowledge representations. The entire field of formal logic in computer science exists precisely to give us a rigorous, machine-processable language. Think of it as the difference between giving a friend directions ("turn left at the big tree") and programming a GPS (precise coordinates, rules, conditions).

This course deals with knowledge and reasoning — topics that are *especially* prone to misunderstanding when described loosely. The goal of this module is to introduce a language that leaves no room for ambiguity: **First Order Logic (FOL)**.

Interestingly, this is not a new idea. When the mathematician **Bertrand Russell** sat down to write *Principia Mathematica* — one of the most ambitious attempts to ground all of mathematics in pure logic — he came to the exact same conclusion: natural language simply would not do. A formal notation was necessary.

We will use logic formalism in **two distinct ways** throughout this course:

1. As a **meta-language** — a tool for *explaining* problems, solutions, and concepts related to knowledge and reasoning.
2. As an **object language** — a tool *in its own right* for actually representing knowledge and performing reasoning.

---

## The Bootstrap Problem: How Do We Talk About Reasoning?

Before we can discuss how reasoning works, we face a curious chicken-and-egg problem. We want to describe systems that represent knowledge and perform automatic reasoning — but we have no language yet to describe those systems! This is what Prof. Chesani calls the **"bootstrap problem"**.

The solution is elegantly simple: **we use symbols**.

Symbols are the most primitive building blocks of any formal language. For our purposes, a symbol is simply a sign — something drawn on a piece of paper or displayed on a slide. We assign meanings to symbols, combine them into sentences, and those sentences can be evaluated as *true* or *false*.

We represent the **act of reasoning** using the following visual convention:

```
Premise 1
Premise 2
──────────────────
Conclusion
```

The horizontal line is the act of reasoning itself. Everything *above* the line is what we know (our premises or hypotheses). Everything *below* the line is what we conclude from that knowledge.

Here are three examples from the slides:

| Premises (above the line) | Conclusion (below the line) |
|---|---|
| "It's raining cats and dogs" | "I'll get wet" |
| "I am hungry" | "I'll have a sandwich" |
| "Every human is mortal" + "Socrates is human" | "Socrates is mortal" |

The classic *Socrates* example is the most famous syllogism in Western philosophy, and it perfectly illustrates the power of this notation: from two known facts, we derive a third fact with absolute certainty.

> **Author's Note:** Notice something important — in the sandwich example, the reasoning seems obvious but is not *logically airtight*. Being hungry does not *guarantee* you will have a sandwich; you might be on a diet, or there might be no sandwiches available. This is a great preview of why formal logic must be so careful. We will revisit this tension between *formal* reasoning and *intuitive* reasoning throughout the course.

---

## Symbols and Their Meanings: The Semiotic Triangle

So we have symbols. But what *is* a symbol, really? How does a sequence of letters — "pizza" — come to represent something in the world?

This question has been debated for centuries in philosophy and linguistics. For this course, we adopt the framework of the **Semiotic Triangle**, originally proposed by **Ogden and Richards**. The triangle has three vertices:

- **Symbol** — the written or spoken sign (e.g., the five characters "p-i-z-z-a")
- **Concept / Thought** — the mental idea the symbol evokes in your mind (your personal, possibly emotional, notion of pizza)
- **Object** — the actual, physical thing in the world (the specific pizza you will eat tonight)

The triangle illustrates that a symbol does *not* point directly to an object in the world. It points to a *concept in someone's mind*, and that concept in turn relates to an object. This indirect relationship has a critical consequence: **different people can associate the same symbol with different concepts**.

---

## The Problem of Interpretation

This is where things get philosophically interesting — and practically problematic. Who decides what a symbol means?

The answer, in principle, is: *anyone*. Every person has the right to assign their own meaning to a symbol. Every person has their own, equally valid interpretation. And here is the problem: **the truth of a sentence depends entirely on what its symbols mean**.

Consider this sentence from a Bob Hope film:
> *"Brave men run in my family."*

- Under interpretation **ℑ₁** — where "run" means "to walk fast" — the sentence is false (the speaker claims to be brave, but does not run).
- Under interpretation **ℑ₂** — where "run" means "to be present in, to belong to" — the sentence might well be true.

Same sentence, same symbols, completely different truth values depending on the interpretation. Now look at the reasoning example again:

```
"It's raining cats and dogs"
────────────────────────────
"I'll get wet"
```

One person concludes "I'll get wet." Another person, taking "cats and dogs" literally, concludes "Let's save the kittens!!!"

Both conclusions are *logically valid* given different interpretations of the premise. This is a disaster for any system that aims to do reliable, automatic reasoning.

Formally, an **interpretation ℑ** is defined as a pair **(D, I)** where:

- **D** is any set of objects — the **domain** (the "world" we are reasoning about)
- **I** is an **interpretation mapping** — a function that maps each non-logical symbol to an element or relation in D

The truth value of any sentence changes with ℑ. So can we ever reason reliably? The answer is yes — but we need to shift our goal.

> **Author's Note:** The concept of interpretation is one of the most important in all of logic and computer science. When you write a program, your variable names are just symbols. The computer doesn't "know" that a variable called `temperature` relates to heat — you define that relationship through the interpretation (the program's logic). Formal logic makes this mapping completely explicit.

---

## Logical Consequence: Reasoning Without Committing to One Interpretation

Instead of asking "is this sentence *true*?" (which requires fixing one interpretation), we ask a more robust question: **is this sentence a logical consequence of these other sentences?**

Here is the formal definition:

Let \(\Gamma = \{F_1, \ldots, F_n\}\) be a set of sentences (the **premises**), and let \(F\) be a sentence (the **conclusion**). \(F\) is a **logical consequence** of \(\Gamma\), written \(\Gamma \models F\), if:

> For **any** possible interpretation, *if all formulas in \(\Gamma\) are true, then \(F\) is also true.*

This is a profound shift. We are no longer committed to one specific interpretation of the world. We are saying: *no matter how you interpret the symbols, if the premises hold, the conclusion must hold too*. This is the rock-solid foundation that makes formal reasoning possible.

---

## Propositional Logic: The Simplest Starting Point

Before diving into First Order Logic, we start with something simpler: **Propositional Logic (PL)**. It is the "hello world" of formal logic — limited in expressive power, but perfect for building intuition.

### Building Blocks

In propositional logic, the basic building blocks are **atoms** (also called atomic formulas), written as \(P_1, P_2, P_3, \ldots\). Each atom represents a simple, indivisible proposition — something that is either true or false. Examples: "It is raining", "The door is open", "Socrates is human."

Atoms can be combined using **logical connectives**:

| Symbol | Name | Meaning |
|---|---|---|
| \(\neg\) | Negation | "not" |
| \(\wedge\) | Conjunction | "and" |
| \(\vee\) | Disjunction | "or" |
| \(\rightarrow\) | Implication | "if … then …" |
| \(\leftrightarrow\) | Equivalence | "if and only if" |
| \(\bot\) | Falsum | always false |

A **well-formed formula (WFF)** is either a single atom, or a combination of WFFs connected by the connectives above. For example, \((P_1 \wedge P_2) \rightarrow P_3\) is a well-formed formula.

### Semantics: What Makes a Formula True or False?

In propositional logic, an **interpretation ℑ** is simply an assignment of truth values (true or false) to each atom. This is the "tertium non datur" principle — there is no third option; every atom is either true or false. *(The slides pose the interesting question of why we only have two truth values — this is worth pondering, as multi-valued logics do exist.)*

Given an interpretation, the truth value of a complex formula is determined mechanically using **truth tables** — one for each connective.

A formula \(G\) is called a **model** for an interpretation ℑ, written \(\mathfrak{I} \models G\), when \(G\) evaluates to true under ℑ.

There are three important special cases:

- **Tautology (valid formula):** \(G\) is true under *every* possible interpretation. Written \(\models G\). Example: \(P \vee \neg P\) ("it is raining or it is not raining").
- **Inconsistent formula:** \(G\) is false under *every* possible interpretation. There is no world where it can hold.
- **Satisfiable (consistent) formula:** \(G\) is true under *at least one* interpretation. This is the most common case.

> **Author's Note:** Do not confuse *invalid* with *inconsistent*. An invalid formula simply fails to be a tautology — it is false in at least one interpretation. An inconsistent formula is false in *all* interpretations. A formula can be invalid but still satisfiable (true in some interpretations). Most useful formulas in practice are satisfiable but not tautologies.

### Decidability in Propositional Logic

Propositional logic has a very nice theoretical property: it is **decidable**. This means there is always a guaranteed, finite procedure to determine whether any formula is valid.

The naive method is simple:
1. Enumerate all possible interpretations.
2. Check the formula under each one.

If a formula has \(n\) atoms, there are exactly \(2^n\) interpretations to check.

But here lies the catch. The slides give a sobering example: a logic network for controlling a **railway station** with a single train track can require **30,000 propositional atoms**. That gives us \(2^{30{,}000}\) interpretations to check — a number so astronomically large that no computer that has ever existed, or could ever exist, could evaluate it in any practical amount of time.

So propositional logic is *theoretically* decidable but *practically* intractable for real-world problems. This motivates us to look for smarter reasoning methods.

---

## Logical Consequence and Logical Equivalence (Formally)

With propositional logic established, we can now state the formal definitions more precisely.

**Logical Consequence:** A formula \(G\) is a logical consequence of \(\{F_1, \ldots, F_n\}\) if and only if, for any interpretation ℑ where \(F_1 \wedge \cdots \wedge F_n\) is true, \(G\) is also true. Written:

\[F_1 \wedge \cdots \wedge F_n \models G\]

**Logical Equivalence:** Two formulas \(F\) and \(G\) are logically equivalent, written \(F \equiv G\), if and only if they have identical truth values under every possible interpretation. Equivalently:

\[F \equiv G \iff (F \models G) \text{ and } (G \models F)\]

There is a rich set of standard equivalences — such as De Morgan's laws, commutativity, associativity, and distributivity — that you will find tabulated in textbooks like *AIMA* (Russell & Norvig, 4th edition, the reference for this course).

---

## Reasoning: From Semantics to Syntax

The notion of logical consequence (\(\models\)) is a *semantic* notion — it talks about truth under all possible interpretations. But checking all possible interpretations is expensive. Can we instead manipulate the *symbols themselves* (the syntax) to derive conclusions?

Yes. This is called **reasoning**, and it is one of the central topics of artificial intelligence.

We write \(KB \vdash_E G\) to say that formula \(G\) can be **derived** from the knowledge base \(KB\) using reasoning method \(E\). The symbol \(\vdash\) (the "turnstile") is the syntactic counterpart of \(\models\).

But not all reasoning methods are equal. Two fundamental properties distinguish good methods from bad ones:

### Soundness
A reasoning method \(E\) is **sound** if every conclusion it derives is actually a logical consequence:

\[(\Gamma \vdash_E \varphi) \Rightarrow (\Gamma \models \varphi)\]

A sound method never gives you a *wrong* answer. If it says \(G\) follows, then \(G\) genuinely follows.

### Completeness
A reasoning method \(E\) is **complete** if it can derive *every* logical consequence:

\[(\Gamma \models \varphi) \Rightarrow (\Gamma \vdash_E \varphi)\]

A complete method never *misses* a valid conclusion. If \(G\) is a logical consequence, the method will eventually find it.

> **Author's Note:** Soundness and completeness are the gold standard for a reasoning method. A method that is both sound and complete is said to be *correct* — it finds all the right answers and only the right answers. In practice, we often face trade-offs. Highly efficient methods may sacrifice completeness; highly expressive logics may sacrifice decidability. Keeping these trade-offs in mind is essential when designing AI systems.

### The Inductivist Turkey: A Warning About Unsound Reasoning

The slides include a wonderful parable attributed to Bertrand Russell (1912) that perfectly illustrates the danger of **unsound reasoning**. It goes like this:

A turkey arrives at a turkey farm. Being a careful, scientific turkey, he doesn't jump to conclusions. He observes that he is fed at 9 a.m. every day — on cold days, warm days, Wednesdays, Thursdays. After many observations, he confidently concludes by *induction*: "I am always fed at 9 a.m."

On the morning of Christmas Eve... the turkey is not fed. He is eaten.

The lesson is stark: **induction** (reasoning from observed cases to a general rule) and **abduction** (reasoning to the best explanation) are common in human thinking, but they are *not sound* reasoning methods in the formal sense. They can yield confident, reasonable-sounding conclusions that turn out to be catastrophically wrong.

For building reliable AI, we need reasoning methods we can *trust*.

---

## Two Powerful Reasoning Strategies

### Reasoning by Deduction

There is a fundamental theorem that connects logical consequence to validity:

**Theorem:** \(F_1 \wedge \cdots \wedge F_n \models G\) *if and only if* \(\models (F_1 \wedge \cdots \wedge F_n) \rightarrow G\).

In words: \(G\) is a logical consequence of the premises if and only if the implication "premises → G" is a *tautology*. So to prove a logical consequence, we can instead prove that a single formula is universally valid.

### Reasoning by Refutation

Another equally powerful theorem gives us a second strategy:

**Theorem:** \(F_1 \wedge \cdots \wedge F_n \models G\) *if and only if* \(F_1 \wedge \cdots \wedge F_n \wedge \neg G\) is **inconsistent**.

This is the **proof by contradiction** principle. To prove that \(G\) follows from the premises, we assume \(G\) is *false* and add that assumption to our knowledge. If this leads to a logical contradiction (the combined formula is inconsistent — false in *all* interpretations), then our assumption was wrong, and \(G\) must indeed be true.

This approach — **refutation** — is the foundation of one of the most important automated reasoning algorithms ever devised.

---

## Resolution: The Engine of Automated Reasoning

In 1965, J. Alan Robinson published a landmark paper: *"A Machine-Oriented Logic Based on the Resolution Principle"* (Journal of ACM, 12(1): 23–41). He showed how refutation could be implemented efficiently using a single, powerful inference rule: **Resolution**.

### Clauses and Literals

The resolution rule operates on formulas written in a special form. A **literal** is either an atom (\(A\)) or its negation (\(\neg A\)). A **clause** is a **disjunction** of literals:

\[A_1 \vee \cdots \vee A_m \vee \neg B_1 \vee \cdots \vee \neg B_k\]

> **Author's Note:** Any propositional formula can be converted into an equivalent formula in **Conjunctive Normal Form (CNF)** — a conjunction of clauses. This conversion is always possible, though it may increase the size of the formula.

### The Resolution Rule

Suppose we have two clauses:
- \(C_1\) contains a literal \(A_k\)
- \(C_2\) contains its negation \(\neg A_k\)

The resolution rule derives a new clause \(C_3\) by taking all literals from \(C_1\) and \(C_2\) *except* the complementary pair \(A_k\) and \(\neg A_k\), and combining them into a single disjunction. \(C_3\) is guaranteed to be a logical consequence of \(C_1\) and \(C_2\).

The truth table in the slides verifies this: for the resolution of \((A \vee B)\) and \((\neg B \vee C)\), the result \((A \vee C)\) is always true whenever both parent clauses are true.

### Refutation + Resolution: The Full Strategy

Combining the refutation theorem with the resolution rule gives us a complete, mechanical procedure for proving logical consequence:

1. **Convert** all formulas in the knowledge base \(KB\) to Conjunctive Normal Form (CNF). Each conjunct becomes a clause.
2. **Negate** the goal formula \(G\) and add \(\neg G\) (also in CNF) to the clause set.
3. **Apply resolution** repeatedly to pairs of clauses, generating new clauses, until either:
   - You derive the **empty clause** \(\bot\) — a contradiction, proving that \(KB \models G\).
   - No new clauses can be derived — meaning \(G\) does not follow from \(KB\).

### A Worked Example

Let's trace through the example from the slides.

**Given knowledge base:**
\[KB = \{(a \rightarrow c \vee d),\ (a \vee d \vee e),\ (a \rightarrow \neg c)\}\]

**Goal:** Prove that \(G = d \vee e\) is a logical consequence of \(KB\).

**Step i — Convert KB to CNF:**
1. \(\neg a \vee c \vee d\) (from \(a \rightarrow c \vee d\))
2. \(a \vee d \vee e\)
3. \(\neg a \vee \neg c\) (from \(a \rightarrow \neg c\))

**Step ii — Negate G and add to clause set:**

4. \(\neg d\)
5. \(\neg e\)

**Step iii — Apply resolution:**

*First iteration:*
- 6: Resolve (1) and (2) on \(a\): \(c \vee d \vee e\)
- 7: Resolve (2) and (3) on \(a\): \(\neg c \vee d \vee e\)
- 8: Resolve (1) and (4) on \(d\): \(\neg a \vee c\)
- 9: Resolve (2) and (4) on \(d\): \(a \vee e\)
- 10: Resolve (2) and (5) on \(e\): \(a \vee d\)
- 11: Resolve (1) and (3) on \(a\): — wait, both contain \(\neg a\); resolve (1) and (3) on \(c\): \(\neg a \vee d\)

*Second iteration:*
- 12: Resolve (10) and (11) on \(a\): \(d\)

*Third iteration:*
- 13: Resolve (4) and (12) on \(d\): **empty clause** ⊥ → **Contradiction!**

The empty clause means we have a contradiction — the assumption that \(\neg G\) is true leads to impossibility. Therefore, \(G = d \vee e\) **is** a logical consequence of \(KB\). ✓

---

## Properties of Resolution in Propositional Logic

How good is resolution as a reasoning method? Let's evaluate it:

| Property | Result | Notes |
|---|---|---|
| **Sound?** | ✅ Yes | If \(KB \cup \{\neg G\} \vdash_{res} \bot\), then \(KB \models G\) |
| **Complete?** | ✅ Yes | If \(KB \models G\), then \(KB \cup \{\neg G\} \vdash_{res} \bot\) |
| **Decidable?** | ✅ Yes | The process always terminates |
| **Efficient?** | ⚠️ Not really | NP-Complete in the worst case |
| **Expressive?** | ❌ Limited | Propositional logic cannot express many real-world knowledge patterns |

The efficiency and expressiveness limitations motivate moving to a more powerful logic.

---

## First Order Logic: The Real Power Tool

Propositional logic is clean and well-behaved, but it has a fatal weakness: it cannot talk about *individual objects* and their *properties* in a general way. Consider the statement "Every human is mortal." In propositional logic, you would need a separate atom for *every single human being* — an impossible task for any non-trivial domain.

**First Order Logic (FOL)** solves this by introducing **variables**, **quantifiers**, **functions**, and **predicates**. It is the language of mathematics, formal philosophy, and the backbone of knowledge representation in AI.

### The Five Building Blocks of FOL

FOL is constructed from five distinct categories of elements:

| Element | Description | Example |
|---|---|---|
| **Constants** | Names for specific objects | `federico`, `rome` |
| **Function symbols** | Operations that map objects to objects | `son_of(federico)` |
| **Variables** | Placeholders for arbitrary objects | `X`, `Y`, `Z` |
| **Predicate symbols** | Relations or properties | `awesome`, `loves` |
| **Logical connectives & quantifiers** | Operators for combining formulas | \(\neg, \wedge, \vee, \rightarrow, \leftrightarrow, \exists, \forall\) |

### Terms: Referring to Objects

A **term** is an expression that refers to an *object* in the domain. Terms are defined recursively:

- Any **constant** is a term (e.g., `federico`)
- Any **variable** is a term (e.g., `X`)
- If \(f\) is a function symbol and \(t_1, \ldots, t_n\) are terms, then \(f(t_1, \ldots, t_n)\) is also a term (e.g., `son_of(federico)`)

The slides give a charming example: `son_of(federico)` refers to the same person as the constant `francesco`. Terms are the nouns of our logical language.

### Atoms: Making Claims About Objects

An **atom** (or atomic formula) applies a predicate symbol to a list of terms:

\[p(t_1, \ldots, t_n)\]

Predicates express properties of, or relationships between, objects. Examples:
- `awesome(federico)` — "Federico is awesome"
- `teenager(francesco)` — "Francesco is a teenager"
- `loves(federico, francesco)` — "Federico loves Francesco"

### Well-Formed Formulas in FOL

Just as in propositional logic, **Well-Formed Formulas (WFFs)** are built recursively:

- Every atom is a WFF.
- If \(\varphi\) and \(\psi\) are WFFs, then \(\neg\varphi\), \(\varphi \wedge \psi\), \(\varphi \vee \psi\), \(\varphi \rightarrow \psi\), and \(\varphi \leftrightarrow \psi\) are WFFs.
- If \(\varphi\) is a WFF and \(X\) is a variable, then \(\exists X\ \varphi\) and \(\forall X\ \varphi\) are WFFs.

The two new operators — **existential quantifier** \(\exists\) ("there exists") and **universal quantifier** \(\forall\) ("for all") — are what give FOL its power.

For example:
- \(\forall X\ (human(X) \rightarrow mortal(X))\) — "Every human is mortal" — works for *all* humans at once.
- \(\exists X\ loves(X, pizza)\) — "Someone loves pizza" — asserts existence without naming who.

A **literal** in FOL is simply an atom or its negation.

---

## Reasoning in First Order Logic

FOL uses the same refutation-and-resolution strategy as propositional logic, but with important extensions to handle variables and quantifiers.

The key additional mechanisms include:
- **Skolemization** — a technique for eliminating existential quantifiers
- **Unification** — an algorithm that finds substitutions making two terms identical
- **Most General Unifier (mgu)** — the most flexible unification possible

The details of these mechanisms are covered in the slides by Prof. Gabbrielli and in *AIMA* chapters 8 and 9.

### A Critical Theoretical Limitation

Unlike propositional logic, **FOL is only semi-decidable** — a result established independently by **Church** and **Turing** (1936), building on **Gödel's completeness theorem** (1930).

What does semi-decidable mean in practice?

- If a formula \(G\) **is** a logical consequence of your axioms, there is a procedure that *will eventually* prove it — in a finite number of steps.
- If \(G\) is **not** a logical consequence, there is **no guarantee** the procedure will ever terminate. It might run forever.

> **Author's Note:** This is a profound result. It means there are questions you can ask a FOL reasoning system for which the system will simply never give you an answer — not because it is broken, but because it is *mathematically impossible* for any algorithm to do so in finite time. This connects to the famous **Halting Problem** in computer science. It is one of the deepest results in the history of logic and computing.

### A Practical Path Forward: Horn Clauses and SLD Resolution

For practical applications, the situation improves significantly when we restrict our attention to a special subset of FOL formulas called **Horn Clauses**. A Horn Clause is a clause with **at most one positive literal**. This restriction sounds limiting, but it is expressive enough to encode a vast range of real-world knowledge.

For Horn Clauses, we can use a more efficient reasoning rule called **SLD resolution** (Linear resolution with Selection function for Definite clauses). This is, in fact, the logical foundation of the programming language **Prolog** — a direct, practical application of everything covered in this module.

---

## Summary of Key Concepts

This module has built up a complete conceptual framework, layer by layer:

- **Symbols and Interpretations** — Symbols get meaning through interpretations. Different interpretations give different truth values.
- **Logical Consequence** (\(\models\)) — A semantic notion: \(G\) follows from premises under *all* interpretations.
- **Propositional Logic** — Atoms, connectives, truth tables. Decidable but not scalable.
- **Tautology / Inconsistency / Satisfiability** — The three fundamental status categories of a formula.
- **Soundness and Completeness** — The two properties a good reasoning method must have.
- **Refutation** — Prove \(G\) by assuming \(\neg G\) leads to contradiction.
- **Resolution** — The syntactic rule for mechanically deriving contradictions from clauses.
- **First Order Logic** — Constants, variables, functions, predicates, quantifiers. Expressive but semi-decidable.

---

## Further Reading

The material in this module is developed further in the following recommended resources:

- **Russell & Norvig, *Artificial Intelligence: A Modern Approach* (AIMA), 4th Edition**
  - Chapter 7: Propositional Logic and Resolution
  - Chapter 8: Introduction to First Order Logic
  - Chapter 9: First Order Inference
- **Slides and course by Prof. Gabbrielli** (for deeper formal treatment of FOL inference)
- **Robinson, J. A. (1965):** "A Machine-Oriented Logic Based on the Resolution Principle." *Journal of the ACM*, 12(1): 23–41. *(The original paper on resolution — a landmark in the history of AI and logic)*

---

*Course by Prof. Ing. Federico Chesani — DISI, University of Bologna*
*Contact: federico.chesani@unibo.it*
