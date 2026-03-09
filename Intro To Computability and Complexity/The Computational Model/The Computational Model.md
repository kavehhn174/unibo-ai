# Introduction to Computability and Complexity
## A Beginner's Guide to Turing Machines and Computational Models

**Course:** Introduction to Computability and Complexity  
**Instructor:** Ugo Dal Lago, University of Bologna (Academic Year 2025-2026)  
**Level:** Introductory  
**Target:** Beginners and New Learners

---

## TABLE OF CONTENTS

1. **Part I: Why Do We Need a Computational Model?**
2. **Part II: Understanding Turing Machines (Informal)**
3. **Part III: Turing Machines Formally Defined**
4. **Part IV: Key Theorems and Concepts**
5. **Part V: Worked Examples and Additional Exercises**
6. **Part VI: Study Guide for Exams**

---

# PART I: WHY DO WE NEED A COMPUTATIONAL MODEL?

## The Problem We Face

### Scenario 1: Proving Something Works ✅ (Easy)
```
Question: "Can we solve this problem?"
Answer: Write a program → Run it on a powerful computer → Show it works!
Difficulty: EASY
```

**Example:** "Can we find the sum of two numbers?"
- Write program → Run it → It works! ✅

### Scenario 2: Proving Something DOESN'T Work ❌ (Very Hard!)
```
Question: "Can this problem be solved at all?"
Answer: ??? (Which machine? Which computer? Which language?)
Difficulty: VERY HARD
```

**Example:** "Can we determine if ANY program will eventually stop or run forever?"
- Try on your laptop? → Maybe it runs forever because your machine is slow
- Try on supercomputer? → Same problem—it might just be a slow algorithm
- **The Real Problem:** Without a standard reference, our answer is meaningless!

## The Solution: Abstract Machine Model

We need a **universal computational model** that is:

### 1️⃣ **Simple Enough**
- Easy to define mathematically
- Easy to prove things about
- Basic operations only (read, write, move)

### 2️⃣ **Powerful Enough**
- Can simulate realistic computers (with reasonable overhead)
- Can express any algorithm we can think of

### 3️⃣ **Universal**
- Everyone agrees on it
- Makes results meaningful across all fields

## The Answer: The Turing Machine (TM)

```
┌─────────────────────────────────────┐
│   UNIVERSAL COMPUTATIONAL MODEL     │
│                                     │
│   The Turing Machine (Proposed      │
│   by Alan Turing in 1936)           │
│                                     │
│   ✓ Simple                          │
│   ✓ Powerful                        │
│   ✓ Universal                       │
└─────────────────────────────────────┘
```

**Why TM?** Because:
- All known computational models (Python, Java, C++, etc.) can be simulated by a TM
- If something cannot be done on a TM, it probably cannot be done at all
- Mathematical proofs about TM are formal and rigorous

---

# PART II: UNDERSTANDING TURING MACHINES (INFORMAL)

## What is a Turing Machine? (Simple Explanation)

Imagine a machine that:
1. Reads from a **tape** (like a very long roll of paper)
2. Follows **instructions** (a program)
3. Uses a **scratchpad** (working memory)
4. Eventually gives an answer: **0 or 1** (No or Yes)

### Visual Representation

```
INPUT TAPE (Binary String)
┌─────────────────────────────────┐
│ 1 │ 0 │ 1 │ 1 │ 0 │ ░ │ ░ │...│
└─────────────────────────────────┘
      ▲
      │ Tape Head (reads here)
      
┌─────────────────────────────────┐
│         INSTRUCTIONS            │
│                                 │
│ Step 1: Read input              │
│ Step 2: Update scratchpad       │
│ Step 3: Move tape head          │
│ Step 4: Change instruction      │
│ Step 5: Go to Step 1            │
└─────────────────────────────────┘

┌─────────────────────────────────┐
│       SCRATCHPAD (Memory)       │
│                                 │
│ Can read and write symbols      │
└─────────────────────────────────┘

Output: 0 or 1 (STOP)
```

## How a Turing Machine Computes

### The 4-Step Cycle (What happens at each step)

```
At Each Step, The Machine Does:

1. READ
   └─> Look at current symbol on tape
   
2. CONSULT INSTRUCTIONS
   └─> "What should I do with this symbol?"
   
3. MODIFY
   └─> Write new symbol (if needed)
   └─> Reposition tape head (left, right, or stay)
   
4. CHANGE STATE
   └─> Go to next instruction
   
Then REPEAT until HALTED
```

### Example: Checking if a String is a Palindrome

**Definition:** A palindrome reads the same forwards and backwards  
**Input:** `101` (is it a palindrome?)  
**Expected Output:** `1` (Yes, it is)

```
Visual Execution:

Step 1: Read first symbol
        Position: 1 │ 0 │ 1
                    ▲
        Read: 1
        Remember: First is 1

Step 2: Move to end
        Position: 1 │ 0 │ 1
                          ▲
        Read: 1
        Check: Last is 1 ✓ (matches first)

Step 3: Move to middle
        Position: 1 │ 0 │ 1
                        ▲
        Read: 0
        Scratchpad: (Middle symbol, OK for palindrome)

RESULT: All symbols match in reverse order = PALINDROME = Output: 1 ✓
```

## Important Concept: Running Time T(n)

### Definition
**T(n) = Maximum number of steps needed for input of length n**

### Explanation with Example

Suppose we have a TM that checks palindromes:
- Input: `1` (length 1) → Takes 3 steps
- Input: `101` (length 3) → Takes 8 steps  
- Input: `10101` (length 5) → Takes 15 steps
- Input: `1010101` (length 7) → Takes 24 steps

**Pattern:** For n-character string, takes approximately 3n steps

**Written as:** T(n) = 3n

### Why This Matters
- **Polynomial Time:** T(n) = 3n, n², n³ (usually considered "fast")
- **Exponential Time:** T(n) = 2ⁿ (usually considered "slow")

---

### Real Example: Parity Check

**Task:** Determine if binary string has even (0) or odd (1) number of 1s

```
Input: 1 1 0 1 0
Count of 1s: 1 + 1 + 0 + 1 + 0 = 3 (ODD)
Output: 1 ✓

Algorithm:
1. Read each symbol one by one
2. Count how many 1s we see
3. If count is even → Output 0
4. If count is odd → Output 1

For string of length n:
- We need to read each position: n steps
- Parity computation: 2 additional steps
- T(n) = n + 2 (approximately n)
```

---

## Machine Representation as Binary String

### Key Insight
> **"A Turing Machine can be encoded as a binary string!"**

### Why is this important?

Since a TM has:
- Finite set of states
- Finite alphabet  
- Finite instructions

We can **encode** all of this as a long binary string!

### Example Encoding

```
TM Machine M = (Σ, Q, δ)

Σ = {0, 1, ░} (alphabet)
Q = {q₁, q₂, q₃} (states)
δ = instructions

Encoded as: ⟨M⟩ = 11010100111010...  (binary string)

Key Point: Any binary string represents SOME TM
           (even if it's not a meaningful one)
```

### Why Does This Matter?

This allows us to have:
1. **Universal Turing Machine (UTM)** - A TM that can simulate ANY other TM
2. **Self-application** - A machine can take another machine's encoding as input
3. **Formal proofs of uncomputability** - We can create paradoxes to prove some functions are impossible

---

## The Universal Turing Machine (UTM)

### Definition
**A Universal Turing Machine (UTM) is a Turing Machine that can simulate any other Turing Machine.**

### Analogy
Think of it like an **interpreter** or **emulator**:
- Your computer can run Windows using VMware on top of macOS
- UTM is like that—it can run any TM's program

### The UTM Theorem (Informal Version)

```
┌──────────────────────────────────────┐
│  UNIVERSAL TURING MACHINE THEOREM    │
├──────────────────────────────────────┤
│                                      │
│  Given:                              │
│  • Machine M (encoded as string ⟨M⟩) │
│  • Input x                           │
│                                      │
│  There exists a Universal TM U that: │
│  U(⟨M⟩, x) = M(x)                   │
│                                      │
│  (U produces same output as M)       │
│                                      │
│  BONUS: If M takes T steps,          │
│         U takes about T·log(T) steps │
│         (small overhead!)            │
│                                      │
└──────────────────────────────────────┘
```

### Example

```
Scenario: We have 3 different TMs

Machine 1: Checks if palindrome
Machine 2: Computes parity
Machine 3: Checks primality

Instead of building 3 separate machines, we have:

Universal TM U + Encoding ⟨M₁⟩  → Can simulate Machine 1
Universal TM U + Encoding ⟨M₂⟩  → Can simulate Machine 2
Universal TM U + Encoding ⟨M₃⟩  → Can simulate Machine 3

This is like having ONE interpreter that can run ANY program!
```

---

# PART III: TURING MACHINES FORMALLY DEFINED

## Formal Definition

A **Turing Machine** is a mathematical object defined as:

### M = (Σ, Q, δ)

Where:

#### 1. **Σ (Sigma) - The Alphabet**
The set of symbols the TM can read and write

```
Example:
Σ = {0, 1, ░} 
   where:
   • 0 = binary digit zero
   • 1 = binary digit one
   • ░ = blank symbol (empty cell)

All Turing Machines must include:
• Blank symbol ░ (represents empty)
• Start symbol ⊢ (marks beginning)
• Binary digits 0, 1
```

#### 2. **Q (States) - The Control**
The set of "instructions" or "program steps" the machine can be in

```
Example:
Q = {q_init, q₁, q₂, q₃, q_halt}

where:
• q_init = initial state (START HERE)
• q₁, q₂, q₃ = intermediate states (processing)
• q_halt = halt state (STOP, we're done)

Think of states like this:
- In state q₁? "Then check if symbol is 0"
- In state q₂? "Then write a 1"
- In state q_halt? "Stop immediately"
```

#### 3. **δ (Delta) - The Transition Function**
"The instruction manual" - describes what to do at each step

```
δ: Q × Σᵏ → Q × Σᵏ⁻¹ × {L, S, R}ᵏ

Read this as: "Given current state and k symbols read,
              output: new state, new symbols, new head positions"

Where:
• Q × Σᵏ    = (current state, k symbols read from k tapes)
• Q         = new state to go to
• Σᵏ⁻¹      = new symbols to write (on k-1 write tapes)
• {L,S,R}ᵏ  = move each head (Left, Stay, or Right)
```

### Complete Formal Description

```
A Turing Machine M = (Σ, Q, δ) where:

✓ Σ  = finite alphabet (symbols the machine uses)
      Must include: ░ (blank), ⊢ (start), 0, 1
      
✓ Q  = finite set of states
      Must include: q_init (start), q_halt (stop)
      
✓ δ  = transition function: Q × Σᵏ → Q × Σᵏ⁻¹ × {L,S,R}ᵏ
      Defines behavior: "If in state q and read symbols s₁...sₖ,
                        then go to state q', write symbols,
                        and move heads"

Special Rule: When in q_halt, machine cannot:
      • Write to tapes
      • Move tape heads
      → Machine STOPS and stays frozen in q_halt
```

---

## Machine Configurations (The State of the System)

### Definition
A **configuration** describes the complete state of the machine at one moment

```
A Configuration C includes:
1. Current state q ∈ Q
2. Contents of all k tapes
3. Positions of all k tape heads
```

### Visual Example

```
Imagine machine with 2 tapes, checking palindrome on "101":

┌─ CONFIGURATION C ─────────────────┐
│                                   │
│ Current State: q₂                 │
│                                   │
│ Tape 1 (Input):                   │
│ ⊢ │ 1 │ 0 │ 1 │ ░ │ ░ │...     │
│     ▲ (head at position 2)        │
│                                   │
│ Tape 2 (Scratchpad):              │
│ ⊢ │ 1 │ ░ │ ░ │ ░ │ ░ │...     │
│     ▲ (head at position 1)        │
│                                   │
│ Configuration notation:           │
│ C = (q₂, "⊢10░░...", "⊢1░░...",  │
│      tape1_head_pos=2,            │
│      tape2_head_pos=1)            │
│                                   │
└───────────────────────────────────┘
```

### Initial Configuration for Input x

```
For input x = "101" ∈ {0,1}*:

Initial Configuration I_x:
─────────────────────────────

1. Current state: q_init (starting state)

2. Tape 1 (input tape, read-only):
   ⊢ │ 1 │ 0 │ 1 │ ░ │ ░ │...
   ▲ (head at start)

3. Tape 2 (scratchpad):
   ⊢ │ ░ │ ░ │ ░ │...
   ▲ (head at start)

All tapes start with head at first position
All non-input tapes start with blank symbol ░
```

### Final Configuration for Output y

```
For output y ∈ {0,1}:

Final Configuration F_y:
──────────────────────

1. Current state: q_halt (MUST be halting state)

2. Output tape contains:
   Result: y │ ░ │ ░ │ ... (followed by blanks)
   
3. Other tapes: (contents don't matter, we stopped)
```

---

## Computation Steps and Paths

### Definition: One Computation Step

```
Configuration C → Configuration D

Means: Apply δ once to get from C to D

Notation: C ⊢ D (reads as "C derives D")
```

### Complete Computation Path

```
For input x and output y:

I_x ⊢ C₁ ⊢ C₂ ⊢ ... ⊢ C_t

where:
• I_x = initial configuration
• C₁, C₂, ... = intermediate configurations
• C_t = final configuration with state q_halt

Each arrow (⊢) represents one application of δ
```

### Notation

```
M(x) = y

Means: "Machine M computes function f such that
        on input x, it outputs y"

M computes function f if:
M(x) = f(x) for ALL inputs x ∈ {0,1}*
```

---

## Computable Functions

### Definition

A function f: {0,1}* → {0,1} is **computable** if there exists a Turing Machine M such that:

**M(x) = f(x)  for all x ∈ {0,1}***

### Example: Palindrome Function

```
f_palindrome(x) = {  1  if x is a palindrome
                  {  0  otherwise

Examples:
f_palindrome("101") = 1  ✓ (palindrome)
f_palindrome("110") = 0  ✗ (not palindrome)
f_palindrome("1") = 1    ✓ (single symbol is palindrome)

Is f_palindrome computable?
YES! We can write a TM that checks palindromes.
```

### Important Note
We do NOT (yet) require the TM to run in any specific time. It just needs to eventually halt and give the right answer!

---

## Efficiency and Runtime

### Definition: Computing in Time T(n)

```
A Turing Machine M computes function f in time T
if for every input x ∈ {0,1}*:

M halts on x in ≤ T(|x|) steps

where |x| = length of input string x
and T: ℕ → ℕ is a time function
```

### Notation and Examples

```
T(n) = n²       → Quadratic time
T(n) = 2ⁿ       → Exponential time
T(n) = 3n       → Linear time
T(n) = n³ + 5n² → Polynomial time
```

### Time Complexity Classes

```
┌─────────────────────────────────────┐
│     COMMON TIME COMPLEXITIES        │
├─────────────────────────────────────┤
│                                     │
│ O(1)     → Constant (instantaneous) │
│ O(log n) → Logarithmic              │
│ O(n)     → Linear                   │
│ O(n²)    → Quadratic                │
│ O(n³)    → Cubic                    │
│ O(2ⁿ)    → Exponential (SLOW!)      │
│                                     │
│ Generally:                          │
│ Polynomial = "fast"                 │
│ Exponential = "slow"                │
│                                     │
└─────────────────────────────────────┘
```

### Real Examples

```
1. PALINDROME CHECK
   T(n) = 3n
   Why: Check n characters, each takes ~3 steps
   Classification: Linear time ✓

2. PARITY COMPUTATION  
   T(n) = n + 2
   Why: Read each of n bits, count them
   Classification: Linear time ✓

3. ADDITION of two n-bit numbers
   T(n) = 3n
   Why: Process each bit pair with carry
   Classification: Polynomial time ✓

4. Trying all subsets of n items
   T(n) = 2ⁿ
   Why: There are 2ⁿ possible subsets
   Classification: Exponential time ✗ (too slow!)
```

---

## Decidable Languages

### Definition

A language L ⊆ {0,1}* is **decidable** (or **recursive**) if there exists a TM M that:

**M(x) = 1  if x ∈ L**  
**M(x) = 0  if x ∉ L**

for all x ∈ {0,1}*

### In Plain English
"A language is decidable if we can write a TM that always correctly answers YES or NO"

### Examples

```
1. PALINDROME LANGUAGE L_pal
   L_pal = {x | x is a palindrome}
   Is it decidable? YES ✓
   Proof: Build TM that checks both directions
   
2. EVEN PARITY LANGUAGE L_even
   L_even = {x | x has even number of 1s}
   Is it decidable? YES ✓
   Proof: Build TM that counts 1s and checks if even
   
3. COMPOSITE NUMBER LANGUAGE L_composite
   L_composite = {x | x represents a composite number}
   Is it decidable? YES ✓
   Proof: Build TM that tries to factor x
```

### Decidable in Time T(n)

```
A language L is decidable in time T if there exists TM M such that:
• For all x ∈ L: M(x) = 1 in ≤ T(|x|) steps
• For all x ∉ L: M(x) = 0 in ≤ T(|x|) steps

Examples:
• L_palindrome is decidable in time T(n) = 3n
• L_parity is decidable in time T(n) = n + 2
• L_primes is decidable in polynomial time
```

---

# PART IV: KEY THEOREMS AND CONCEPTS

## Theorem 1: Robustness of Turing Machines

### The Problem
Our TM definition has many arbitrary choices:
- Tape alphabet: We chose {0, 1, ░}, but could use more symbols
- Number of tapes: We chose k, but could use 1 or 100
- Head movement: We allow L, S, R, but could limit choices
- Direction: Tapes can extend in one direction or both

### The Answer: Robustness

```
┌─────────────────────────────────────────────────┐
│ ROBUSTNESS THEOREM                              │
├─────────────────────────────────────────────────┤
│                                                 │
│ Any two "reasonable" definitions of Turing      │
│ Machines can simulate each other with only      │
│ POLYNOMIAL OVERHEAD in time!                    │
│                                                 │
│ Formally: If model A simulates model B,         │
│ and B takes T(n) steps,                         │
│ then A takes O(T(n) · p(n)) steps               │
│ for some polynomial p(n)                        │
│                                                 │
│ Example: p(n) = n², n³, etc.                    │
│ (NOT exponential!)                              │
│                                                 │
└─────────────────────────────────────────────────┘
```

### What Does This Mean?

```
Different TM Models:
─────────────────

Model 1: Single tape, binary alphabet
Model 2: 3 tapes, larger alphabet
Model 3: Infinite tapes in both directions

Robustness Says:
───────────────
Any problem solvable in polynomial time on Model 1
is solvable in polynomial time on Model 2, 3, etc.

The constant might change, but polynomial degree stays same!

This is WHY we don't worry about exact TM definition.
```

---

## Theorem 2: The Universal Turing Machine (UTM)

### Formal Statement

```
┌──────────────────────────────────────────────┐
│ UNIVERSAL TURING MACHINE THEOREM             │
├──────────────────────────────────────────────┤
│                                              │
│ There exists a Turing Machine U such that   │
│ for all x, σ ∈ {0,1}*:                      │
│                                              │
│    U(σ, x) = M_σ(x)                         │
│                                              │
│ where M_σ is the TM encoded by string σ     │
│                                              │
│ Moreover (efficiency):                       │
│ If M_σ halts on x in T steps,               │
│ then U halts on (σ,x) in C·T·log(T) steps   │
│ where C depends only on M_σ                  │
│                                              │
└──────────────────────────────────────────────┘
```

### Explanation

```
Input to U:
• σ = binary encoding of machine M
• x = binary input to M

U will:
1. Decode σ to understand what M is
2. Simulate M running on input x
3. Output what M would output

Speed: If simulation takes T steps,
       overhead is only logarithmic in T
       → Very efficient!
```

### Why This Matters

```
1. PROGRAMMING LANGUAGES
   Just like we have interpreters (like Python interpreter),
   we have the UTM—the universal interpreter for algorithms

2. COMPUTABILITY THEORY
   We can ask: "What about the UTM itself as input?"
   This leads to fundamental proofs!

3. SELF-REFERENCE
   Can we ask a machine to simulate itself?
   Answer: YES! This leads to paradoxes and proofs
           of uncomputability
```

---

## Theorem 3: Uncomputable Functions Exist

### The Surprising Result

```
┌───────────────────────────────────────────┐
│ UNCOMPUTABILITY THEOREM                   │
├───────────────────────────────────────────┤
│                                           │
│ There exists a function f_uc that is      │
│ NOT computable by ANY Turing Machine      │
│                                           │
│ Definition:                               │
│ f_uc(σ) = { 0  if M_σ(σ) = 1             │
│           { 1  otherwise                  │
│                                           │
│ (Roughly: "Does this machine NOT accept  │
│  its own encoding?")                      │
│                                           │
└───────────────────────────────────────────┘
```

### Why is f_uc Uncomputable? (Proof Idea)

```
Assume: f_uc is computable
        There exists TM M_uc that computes f_uc

Then: M_uc(σ) = f_uc(σ) for all σ

Special case: Let σ = encoding of M_uc itself

Then: M_uc(⟨M_uc⟩) = f_uc(⟨M_uc⟩)
                    = 0 if M_uc(⟨M_uc⟩) = 1
                    = 1 if M_uc(⟨M_uc⟩) ≠ 1

This means: M_uc(⟨M_uc⟩) = 0 if M_uc(⟨M_uc⟩) = 1
            M_uc(⟨M_uc⟩) = 1 if M_uc(⟨M_uc⟩) = 0

CONTRADICTION! ✗

Therefore: f_uc is NOT computable ✓
```

### Similar to Russell's Paradox

```
Russell's Paradox (logic):
Set R = {all sets that don't contain themselves}
Question: Does R contain itself?
If yes → R doesn't contain itself (contradiction)
If no → R contains itself (contradiction)

Conclusion: No such set can exist!

Similarly:
Function that checks "does this machine accept itself?"
Cannot exist, because it leads to paradox!
```

---

## Theorem 4: The Halting Problem

### Definition

```
The HALTING PROBLEM:
─────────────────

Given: TM encoded as σ, input string x

Question: Does M_σ halt on input x?

Formally:
halt(σ, x) = { 1  if M_σ halts on input x
             { 0  if M_σ runs forever on input x
```

### The Problem is Undecidable

```
┌─────────────────────────────────────────────┐
│ HALTING PROBLEM THEOREM                     │
├─────────────────────────────────────────────┤
│                                             │
│ The Halting Problem is UNDECIDABLE          │
│                                             │
│ There is NO Turing Machine that can         │
│ determine whether an arbitrary program      │
│ halts or runs forever                       │
│                                             │
└─────────────────────────────────────────────┘
```

### Why Can't We Solve the Halting Problem?

```
Informal Proof:

Assume: There exists TM H that solves halting problem
        H(σ, x) = 1 if M_σ halts on x
        H(σ, x) = 0 if M_σ runs forever

Then: Build new machine M_bad:

      On input σ:
      1. Run H(σ, σ)  [Ask: does M_σ halt on itself?]
      2. If H returns 0: HALT (return 1)
      3. If H returns 1: LOOP FOREVER

Now apply M_bad to its own encoding ⟨M_bad⟩:

      M_bad(⟨M_bad⟩) halts?
      
      Case 1: M_bad halts on ⟨M_bad⟩
              → H returns 1 (yes, it halts)
              → M_bad loops forever (contradiction!)
              
      Case 2: M_bad loops forever on ⟨M_bad⟩
              → H returns 0 (no, it doesn't halt)
              → M_bad halts (contradiction!)

PARADOX! Therefore H cannot exist!
```

### Practical Implications

```
Why This Matters:

❌ Cannot build automatic debugger that always detects infinite loops
❌ Cannot write program that checks "does this code run forever?"
❌ Cannot automatically prove termination of arbitrary programs

This isn't a limitation of our skills—it's mathematically IMPOSSIBLE!

Connected to Gödel's Incompleteness Theorem:
In any consistent logical system, there are true statements
that cannot be proven within the system.
```

---

## Theorem 5: Rice's Theorem

### Definition: Semantic Properties

```
A language L is SEMANTIC if:

  If machine M and machine N compute the SAME function,
  then: M ∈ L  ⟺  N ∈ L
  
(i.e., whether M is in L depends only on WHAT function it computes,
       not HOW it computes it)
```

### Examples of Semantic Properties

```
1. "Set of machines computing the identity function"
   Semantic? YES ✓
   Why: Only depends on what function is computed

2. "Set of machines with exactly 5 states"
   Semantic? NO ✗
   Why: Depends on HOW it's written, not what it computes

3. "Set of machines that halt on all inputs"
   Semantic? YES ✓
   Why: About property of function (total), not implementation

4. "Set of machines that never output 0"
   Semantic? YES ✓
   Why: About output behavior, not implementation
```

### Rice's Theorem

```
┌──────────────────────────────────────────────┐
│ RICE'S THEOREM                               │
├──────────────────────────────────────────────┤
│                                              │
│ Every non-trivial semantic property of       │
│ Turing Machines is UNDECIDABLE              │
│                                              │
│ Non-trivial means:                           │
│ • Not ALL machines satisfy the property      │
│ • Not NO machines satisfy the property       │
│                                              │
│ Formally:                                    │
│ If L is semantic and ∅ ⊂ L ⊂ {all TMs},     │
│ then L is undecidable                        │
│                                              │
└──────────────────────────────────────────────┘
```

### Examples: What We CANNOT Decide

```
❌ "Does this machine compute a total function?" (runs on all inputs)
❌ "Does this machine compute the same function as this other machine?"
❌ "Does this machine halt on exactly half its inputs?"
❌ "Does this machine run in polynomial time?"

All undecidable by Rice's Theorem!
```

### Why is Rice's Theorem Important?

```
It shows:
────────

Nearly ALL interesting properties about WHAT programs compute
are UNDECIDABLE!

We CAN decide:
✓ Number of states
✓ Length of description
✓ Syntactic properties

We CANNOT decide:
❌ What function is computed
❌ Whether function terminates
❌ Whether function is total
❌ Whether output has property P
```

---

## Diophantine Equations and MDPR Theorem

### Definition: Diophantine Equation

```
A DIOPHANTINE EQUATION is a polynomial equation
with integer coefficients and finitely many unknowns

Examples:
─────────

1. x² + 3y = 2x + 1
   (unknowns: x, y)

2. x⁴ + 3x³ - y + z = 12
   (unknowns: x, y, z)

3. x² + y² = z²
   (Pythagorean equation)

Question: Does the equation have an INTEGER solution?

For x² + y² = z²: YES ✓ (e.g., 3² + 4² = 5²)
For x² + 3y = 2x + 1: YES ✓ (e.g., x=2, y=3)
```

### The MDPR Theorem (Matiyasevich-Davis-Putnam-Robinson)

```
┌────────────────────────────────────────────┐
│ MDPR THEOREM                               │
├────────────────────────────────────────────┤
│                                            │
│ The problem of determining whether an      │
│ arbitrary Diophantine equation has an      │
│ integer solution is UNDECIDABLE            │
│                                            │
│ Formally:                                  │
│ There is NO Turing Machine that can        │
│ determine for every Diophantine equation   │
│ whether it has an integer solution         │
│                                            │
│ This resolved Hilbert's 10th Problem       │
│ (posed in 1900)                            │
│                                            │
└────────────────────────────────────────────┘
```

### Why is This Surprising?

```
Initially hoped that:
"Every well-posed mathematical problem
 either has a solution or doesn't,
 and we can decide which"

MDPR shows:
"Some problems about mathematical objects
 (like finding integer solutions)
 are fundamentally undecidable"

No algorithm exists, even in principle!
```

---

# PART V: WORKED EXAMPLES AND ADDITIONAL EXERCISES

## Exercise Set 1: Understanding the Basics

### Exercise 1.1: Tape Representation

**Problem:** Draw the configuration of a 2-tape TM for the following scenario:

State: q₂  
Tape 1 (read-only input): 1 0 1 1  
Tape 2 (scratchpad): 1 0  
Tape 1 head: at position 3  
Tape 2 head: at position 1  

**Solution:**

```
┌─────────────────────────────────────┐
│        Configuration C              │
├─────────────────────────────────────┤
│                                     │
│ Current State: q₂                   │
│                                     │
│ Tape 1 (Input tape):                │
│ ⊢ │ 1 │ 0 │ 1 │ 1 │ ░ │ ░ │...    │
│         1   2   3 ▲ 4   5           │
│                (head)               │
│                                     │
│ Tape 2 (Scratchpad):                │
│ ⊢ │ 1 │ 0 │ ░ │ ░ │...            │
│     ▲           (head)              │
│     1                               │
│                                     │
│ Notation: C = (q₂, tape1[1:4],     │
│                     tape2[1:2],     │
│                     pos1=3,         │
│                     pos2=1)         │
│                                     │
└─────────────────────────────────────┘
```

---

### Exercise 1.2: Identifying Computable Functions

**Problem:** Which of these are computable? (YES/NO with brief reasoning)

a) f(x) = 1 if x is even number, 0 if odd  
b) f(x) = 1 if x is a prime, 0 otherwise  
c) f(x) = the 10th digit of π  
d) f_uc(x) = 0 if M_x(x) = 1, 1 otherwise  

**Solution:**

```
a) YES ✓ Computable
   Reason: Can check last bit. Even = last bit is 0
   T(n) = O(1) or O(n) depending on representation

b) YES ✓ Computable
   Reason: Can check divisibility by all numbers up to √x
   Algorithm: Trial division
   T(n) = O(√x) = O(2^(n/2)) where n = log₂(x)

c) YES ✓ Computable
   Reason: π can be computed to arbitrary precision
   Algorithm: Spigot algorithm or other
   T(n) = polynomial in number of digits

d) NO ✗ Not computable
   Reason: This is the uncomputability function f_uc
   We proved this is uncomputable!
   (This is THE classic example of uncomputable function)
```

---

### Exercise 1.3: Time Complexity Classification

**Problem:** Classify each time function:

a) T(n) = 5n + 10  
b) T(n) = n² + n  
c) T(n) = 2ⁿ  
d) T(n) = n log n  
e) T(n) = 100  

**Solution:**

```
Classification:
──────────────

a) T(n) = 5n + 10
   Classification: LINEAR
   Why: Highest power of n is n¹
   Notation: O(n)
   Speed: FAST ✓

b) T(n) = n² + n
   Classification: QUADRATIC
   Why: Highest power is n²
   Notation: O(n²)
   Speed: Medium (for large n)

c) T(n) = 2ⁿ
   Classification: EXPONENTIAL
   Why: n appears in exponent
   Notation: O(2ⁿ)
   Speed: VERY SLOW ✗
   
   Example: n=20 → 2²⁰ = 1,048,576 steps
            n=30 → 2³⁰ = 1,073,741,824 steps (1 billion+!)

d) T(n) = n log n
   Classification: LINEARITHMIC (between linear and quadratic)
   Why: n · log(n)
   Notation: O(n log n)
   Speed: FAST ✓
   
   Example: Mergesort, many divide-and-conquer algorithms

e) T(n) = 100
   Classification: CONSTANT
   Why: Doesn't depend on n
   Notation: O(1)
   Speed: FASTEST ✓ (same time regardless of input size)
```

**Summary Table:**

```
┌──────────────────────────────────────┐
│ TIME COMPLEXITY RANKING (FAST→SLOW)  │
├──────────────────────────────────────┤
│                                      │
│ O(1)      Constant    ✓✓✓✓✓          │
│ O(log n)  Logarithmic ✓✓✓✓           │
│ O(n)      Linear      ✓✓✓            │
│ O(n log n) Linearithmic ✓✓✓          │
│ O(n²)     Quadratic   ✓✓             │
│ O(n³)     Cubic       ✓              │
│ O(2ⁿ)     Exponential ✗✗             │
│ O(n!)     Factorial   ✗✗✗            │
│                                      │
└──────────────────────────────────────┘
```

---

## Exercise Set 2: Working with Configurations

### Exercise 2.1: Initial Configuration

**Problem:** Write the initial configuration for input x = "11010"

Assume: 2-tape TM, tape 1 is read-only input, tape 2 is scratchpad

**Solution:**

```
Initial Configuration I_x for x = "11010":
─────────────────────────────────────────

State: q_init (by definition, initial state)

Tape 1 (input, read-only):
┌─────────────────────────────┐
│ ⊢ │ 1 │ 1 │ 0 │ 1 │ 0 │ ░ │
└─────────────────────────────┘
  ▲
  head at position 1 (start symbol)

Tape 2 (scratchpad, initially blank):
┌─────────────────────────────┐
│ ⊢ │ ░ │ ░ │ ░ │ ░ │ ░ │ ░ │
└─────────────────────────────┘
  ▲
  head at position 1 (start symbol)

Notation:
I_"11010" = (q_init,
             tape1 = "⊢11010░░...",
             tape2 = "⊢░░░░░░...",
             head1_pos = 1,
             head2_pos = 1)
```

---

### Exercise 2.2: Transitions

**Problem:** Given transition function δ:

```
δ(q₁, '1', '░') = (q₂, '1', R, S)

Meaning: If in state q₁, read '1' on tape 1, read '░' on tape 2,
         then: go to state q₂, write '1' on tape 2,
               move tape 1 right, keep tape 2 in place
```

Starting from configuration:
```
State: q₁
Tape 1: ⊢ │ 1 │ 1 │ 0
Tape 1 head: position 2 (pointing at first '1')
Tape 2: ⊢ │ ░ │ ░
Tape 2 head: position 1 (pointing at ⊢)
```

**Find:** The next configuration after one transition

**Solution:**

```
Before Transition (Configuration C):
────────────────────────────────────

State: q₁

Tape 1: ⊢ │ 1 │ 1 │ 0
Head:       ▲ (position 2, reads '1')

Tape 2: ⊢ │ ░ │ ░
Head:   ▲ (position 1, reads '░')

Apply δ(q₁, '1', '░'):
──────────────────────
Action: (q₂, '1', R, S)

1. Change state: q₁ → q₂
2. Write '1' on tape 2 at head position
3. Move tape 1 head RIGHT (R)
4. Keep tape 2 head in place (S)

After Transition (Configuration C'):
─────────────────────────────────────

State: q₂

Tape 1: ⊢ │ 1 │ 1 │ 0
Head:           ▲ (position 3, moved right)

Tape 2: ⊢ │ 1 │ ░
Head:   ▲ (position 1, stayed in place)
         (but we just wrote '1' here)

Summary:
C = (q₁, tapes[...], heads[2,1])
      ↓ (apply δ)
C' = (q₂, tapes[...], heads[3,1])
```

---

## Exercise Set 3: Palindrome Checker (Detailed Example)

### Exercise 3.1: Palindrome Checker Informal Algorithm

**Problem:** Explain how a TM would check if "101" is a palindrome

**Solution:**

```
Algorithm: Check Palindrome
───────────────────────────

Input: Binary string x
Question: Is x a palindrome?

Informal Steps:

Step 1: START AT BEGINNING
        Head position = 1
        Read first symbol (record it: is it 0 or 1?)

Step 2: FIND END
        Move right until we see blank ░
        This marks the end of string

Step 3: MOVE BACK ONE
        Move left one position
        This is the last symbol

Step 4: COMPARE
        First symbol vs Last symbol
        If different → NOT palindrome (output 0)
        If same → CONTINUE

Step 5: MOVE INWARD
        Go back to start, move right past first symbol
        Go to second symbol from end
        
Step 6: REPEAT
        Repeat steps 4-5 until all symbols checked

Step 7: OUTPUT
        If all match → OUTPUT 1 (palindrome)


Example Execution for x = "101":
─────────────────────────────────

Tape: ⊢ │ 1 │ 0 │ 1 │ ░

Round 1:
First:  ▲ reads '1'
        Move right, right, right...
Last:               ▲ reads '1'
Compare: '1' = '1' ✓ Match!

Round 2:
First:      ▲ reads '0'
        Move right...
Last:       ▲ reads '0'
Compare: '0' = '0' ✓ Match!

Middle: ▲ (only '1' in middle for odd length)
Result: ALL matched → OUTPUT 1 ✓ (palindrome)
```

---

### Exercise 3.2: Formal State Machine for Palindrome Checker

**Problem:** Define states Q for a simple palindrome checker

**Solution:**

```
State Machine for Palindrome Checker:
─────────────────────────────────────

Q = {q_init, q_find_end, q_compare, q_move_inward, q_accept, q_reject, q_halt}

State Descriptions:
──────────────────

q_init:
  Job: Read first character, remember it
  Action: "Record first symbol in scratchpad"
  Next: q_find_end

q_find_end:
  Job: Move to end of string
  Action: "Move right until we see blank ░"
  Next: (when blank found) q_compare

q_compare:
  Job: Compare last symbol with remembered first symbol
  Action: Read symbol at current position
          Compare with scratchpad
          If match: q_move_inward
          If no match: q_reject

q_move_inward:
  Job: Move pointers closer to middle
  Action: Move first pointer right,
          Move last pointer left,
          Check if they passed each other
  Next: If not crossed: q_compare
        If crossed: q_accept (all matched!)

q_accept:
  Job: Output YES (1)
  Action: Write '1', move to halt

q_reject:
  Job: Output NO (0)
  Action: Write '0', move to halt

q_halt:
  Job: Stop computation
  Action: Freeze in this state (cannot do anything)
  RESULT: Computation complete
```

---

## Exercise Set 4: Creating Your Own Exercise

### Exercise 4.1: Parity Checker

**Original Example:**

**Problem:** Determine if string has ODD number of 1s
```
Input: "11010"
Count of 1s: 3 (ODD)
Output: 1 ✓

Input: "1010"
Count of 1s: 2 (EVEN)
Output: 0 ✓
```

**Your Exercise:** Parity Check with Counter

**Problem:** Modify the parity checker to output the actual COUNT of 1s (not just odd/even)

```
Input: "11010"
Output: 3 (there are three 1s)

Input: "1010"
Output: 2 (there are two 1s)

Input: "111"
Output: 3 (there are three 1s)
```

**Solution:**

```
Algorithm: Count 1s
───────────────────

Step 1: START
        Set counter = 0 (in scratchpad)
        Position = first symbol

Step 2: READ
        Read current symbol

Step 3: CHECK
        If symbol = '1':
          Increment counter
        If symbol = '0':
          Leave counter alone

Step 4: ADVANCE
        Move right to next symbol

Step 5: STOP CHECK
        If next symbol = blank ░:
          → Go to Step 6
        Else:
          → Go back to Step 2

Step 6: OUTPUT
        Write counter value as result
        Done!

Example Execution for x = "11010":
──────────────────────────────────

Position  Symbol  Counter  Action
──────────────────────────────────
   1       1        0      Read '1' → +1 → counter = 1
   2       1        1      Read '1' → +1 → counter = 2
   3       0        2      Read '0' → no change → counter = 2
   4       1        2      Read '1' → +1 → counter = 3
   5       ░        3      Blank found! → STOP

OUTPUT: counter = 3 ✓
```

---

### Exercise 4.2: Even/Odd Binary Number Classifier

**Original Example:**

**Problem:** Determine if binary number is even or odd
```
"110" = 6 in decimal = EVEN = Output: 0
"111" = 7 in decimal = ODD = Output: 1
```

**Your Exercise:** Create a Similar Problem

**Problem:** Determine if a binary number is DIVISIBLE BY 4

```
Hint: A binary number is divisible by 4 if last 2 bits are "00"

Examples:
"1100" = 12 = divisible by 4? YES → Output: 1
"1010" = 10 = divisible by 4? NO → Output: 0
"10000" = 16 = divisible by 4? YES → Output: 1
```

**Solution:**

```
Algorithm: Check Divisibility by 4
───────────────────────────────────

Key Insight: Binary numbers have structure
  Divisible by 2  ← if last bit is 0
  Divisible by 4  ← if last 2 bits are 00
  Divisible by 8  ← if last 3 bits are 000
  etc.

Algorithm for "Divisible by 4":

Step 1: FIND END
        Move to end of string (before blank)

Step 2: CHECK LAST BIT
        If last bit ≠ 0:
          → NOT divisible by 4 → Output: 0

Step 3: CHECK SECOND-TO-LAST BIT
        Move left one position
        If this bit ≠ 0:
          → NOT divisible by 4 → Output: 0

Step 4: SUCCESS
        Both last two bits are 0
        → Divisible by 4 → Output: 1

Example Execution for x = "1100":
──────────────────────────────────

Tape: ⊢ │ 1 │ 1 │ 0 │ 0 │ ░

Position of last bit:
Tape:                   ▲ (before blank ░)
Read: '0' ✓ (divisible by 2)

Position of second-to-last bit:
Move left:
Tape:               ▲
Read: '0' ✓ (divisible by 4!)

OUTPUT: 1 (YES, divisible by 4) ✓

Decimal check: 1100₂ = 1×8 + 1×4 + 0×2 + 0×1 = 12
              12 ÷ 4 = 3 ✓
```

**Time Complexity Analysis:**

```
T(n) = ?

Operations needed:
1. Move to end: n steps
2. Check last bit: 1 step
3. Move back one: 1 step
4. Check second-to-last bit: 1 step

Total: T(n) = n + 3 = O(n)

Classification: LINEAR TIME ✓
```

---

### Exercise 4.3: Majority Bit Checker

**Your Exercise:** Create another original problem

**Problem:** Given a binary string, determine if there are MORE 1s than 0s

```
Examples:
"111" → three 1s, zero 0s → More 1s → Output: 1
"110" → two 1s, one 0 → More 1s → Output: 1
"101" → two 1s, one 0 → More 1s → Output: 1
"100" → one 1, two 0s → More 0s → Output: 0
"1010" → two 1s, two 0s → EQUAL → Output: 0
```

**Solution:**

```
Algorithm: Count and Compare
──────────────────────────────

Step 1: COUNT ALL 1S
        Move through string, count each '1'
        Store count_1s in scratchpad

Step 2: COUNT ALL 0S
        Move through string again, count each '0'
        Store count_0s in scratchpad

Step 3: COMPARE
        Compare count_1s with count_0s
        
        If count_1s > count_0s:
          OUTPUT: 1 (more 1s)
        Else:
          OUTPUT: 0 (not more 1s)

Example Execution for x = "110":
─────────────────────────────────

Round 1: Count 1s
Position 1: '1' → count = 1
Position 2: '1' → count = 2
Position 3: '0' → count = 2 (no change)
Result: count_1s = 2

Round 2: Count 0s
Position 1: '1' → count = 0 (no change)
Position 2: '1' → count = 0 (no change)
Position 3: '0' → count = 1
Result: count_0s = 1

Round 3: Compare
count_1s = 2
count_0s = 1
2 > 1? YES ✓

OUTPUT: 1 (More 1s than 0s) ✓
```

**Time Complexity Analysis:**

```
T(n) = 2n + comparison

We scan the string twice: 2n steps
Comparison: constant time

T(n) = 2n + O(1) = O(n)

Classification: LINEAR TIME ✓
```

---

## Exercise Set 5: Understanding Undecidability

### Exercise 5.1: The Halting Problem

**Problem:** Explain why we cannot write a program that detects infinite loops

**Solution:**

```
Why Halting Problem is Undecidable:
─────────────────────────────────────

Proof by Contradiction:
───────────────────────

Assume: There EXISTS a program H that solves halting
        H(M, x) = 1 if M halts on x
        H(M, x) = 0 if M loops forever

Then: We can build a paradoxical program B:

      Program B(M):
        1. Run H(M, M)  [Ask: Does M halt on itself?]
        2. If H says "loops forever" (0): HALT now
        3. If H says "halts" (1): LOOP FOREVER

Now ask: What happens when we run B on itself?
         B(B) = ?

Case 1: B(B) halts
        Then H(B,B) = 1
        Then B(B) loops forever (step 3)
        CONTRADICTION! ✗

Case 2: B(B) loops forever
        Then H(B,B) = 0
        Then B(B) halts (step 2)
        CONTRADICTION! ✗

Both cases lead to contradictions!
Therefore: Our assumption must be false
           No such program H can exist!
```

**Intuitive Explanation:**

```
The problem with H:

┌─────────────────────────────────┐
│  Machine H (Halting detector)   │
├─────────────────────────────────┤
│ Input: (Program M, Input x)     │
│ Output: 1 = halts               │
│         0 = loops forever       │
└─────────────────────────────────┘

We can create a trickster program:
"Do the OPPOSITE of what H predicts"

Trickster(M):
  Ask H: "Does M halt?"
  If H says YES: LOOP FOREVER
  If H says NO: HALT

When we ask about Trickster(Trickster):
  "Does Trickster halt on itself?"
  
Trickster does the OPPOSITE of what H says!
This creates an unsolvable paradox.

It's like the liar's paradox:
"This statement is false"
If true → false (contradiction)
If false → true (contradiction)
```

---

### Exercise 5.2: Rice's Theorem Application

**Problem:** Which of these properties are decidable? (Use Rice's Theorem)

a) "Does this machine run in polynomial time?"
b) "Does this machine have exactly 10 states?"
c) "Does this machine halt on input '000'?"
d) "Does this machine compute the identity function?"

**Solution:**

```
Rice's Theorem Recap:
────────────────────

Non-trivial SEMANTIC properties are undecidable

where:
• Semantic = depends only on what function is computed
• Non-trivial = not all machines, not no machines

Analysis:
──────────

a) "Does this machine run in polynomial time?"

   Semantic? YES ✓ (about function behavior)
   Non-trivial? YES ✓ (some run polynomial, some don't)
   Decidable? NO ✗ (by Rice's Theorem)
   
   Why: This is a semantic property of runtime behavior

b) "Does this machine have exactly 10 states?"

   Semantic? NO ✗ (about HOW it's written, not WHAT it computes)
   Why: Two machines computing same function can differ in states
   
   This is a SYNTACTIC property
   Decidable? MAYBE ✓ (this is not covered by Rice's Theorem)
   
   Answer: YES, decidable (just count the states in the description)

c) "Does this machine halt on input '000'?"

   Semantic? NO ✗ (depends on specific behavior)
   Why: This is a very specific input behavior
   
   However: This is harder than halting problem!
   Decidable? NO ✗ (undecidable for other reasons)

d) "Does this machine compute the identity function?"

   Semantic? YES ✓ (the function IS the identity)
   Non-trivial? YES ✓ (not all functions, not none)
   Decidable? NO ✗ (by Rice's Theorem)
   
   Why: Even though identity is specific, checking if any
        arbitrary machine computes it is undecidable

Summary:
────────

┌────────────────────┬──────────────┬────────────┐
│ Property           │ Semantic?    │ Decidable? │
├────────────────────┼──────────────┼────────────┤
│ a) Polynomial time │ Yes (SEMANTIC) │ NO ✗      │
│ b) 10 states       │ No (SYNTAX)  │ YES ✓      │
│ c) Halts on '000'  │ No           │ NO ✗       │
│ d) Identity func   │ Yes (SEMANTIC) │ NO ✗      │
└────────────────────┴──────────────┴────────────┘
```

---

# PART VI: STUDY GUIDE FOR EXAMS

## Key Concepts to Memorize

### 1. Why We Need a Computational Model

**Memorize:**
- Can't just use specific computers (results won't generalize)
- Need universal model (everyone agrees on)
- Model must be simple (for proofs) and powerful (can simulate reality)
- Answer: Turing Machine ✓

**Be able to:**
- Explain why negative results are hard without a model
- Give examples of positive vs negative computational results

---

### 2. Turing Machine Components

**Memorize the Triple:**

```
M = (Σ, Q, δ)

Σ = Alphabet (symbols machine uses)
    Must include: ░ (blank), ⊢ (start), 0, 1

Q = States (control/program flow)
    Must include: q_init (start), q_halt (stop)

δ = Transition function (instructions)
    δ: Q × Σᵏ → Q × Σᵏ⁻¹ × {L,S,R}ᵏ
```

**Be able to:**
- Describe what each component does
- Explain with examples
- Write formal notation

---

### 3. Configurations

**Memorize:**

Configuration C = (state, tape_contents, head_positions)

**Be able to:**
- Draw initial configuration I_x for given input
- Apply transition δ to get next configuration
- Trace through small examples

---

### 4. Computable Functions and Decidable Languages

**Memorize:**

```
Function f is COMPUTABLE if:
├─ There exists TM M such that
├─ M(x) = f(x) for ALL inputs x
└─ (doesn't matter how long it takes)

Language L is DECIDABLE if:
├─ There exists TM M such that
├─ M(x) = 1 if x ∈ L
├─ M(x) = 0 if x ∉ L
└─ For ALL inputs x
```

**Be able to:**
- Distinguish "computable" from "computable in polynomial time"
- Give examples of decidable languages
- Explain informally why some languages are decidable

---

### 5. Time Complexity

**Memorize the Hierarchy:**

```
O(1) < O(log n) < O(n) < O(n log n) < O(n²) < O(n³) < O(2ⁿ)

FAST ←─────────────────────────────→ SLOW
```

**Be able to:**
- Classify functions by complexity class
- Understand polynomial vs exponential
- Give algorithms for each class

---

### 6. Major Theorems

**Must Memorize:**

```
1. ROBUSTNESS OF TM
   Different reasonable TM definitions can simulate each other
   with only polynomial overhead

2. UNIVERSAL TURING MACHINE
   ∃ machine U that can simulate any machine M
   U(⟨M⟩, x) = M(x)
   Overhead: O(T log T) if M takes T steps

3. UNCOMPUTABLE FUNCTIONS EXIST
   f_uc(σ) = 0 if M_σ(σ) = 1, else 1
   This function is NOT computable!
   Proof: Self-reference leads to paradox

4. HALTING PROBLEM IS UNDECIDABLE
   Cannot determine if arbitrary program halts
   Proof: Similar to uncomputability

5. RICE'S THEOREM
   Every non-trivial semantic property is undecidable
   (Can't check if programs compute specific functions)
```

**Be able to:**
- State each theorem precisely
- Understand intuitive explanation
- Know at least one consequence

---

### 7. Diophantine Equations

**Memorize:**

```
DIOPHANTINE EQUATION:
   Polynomial with integer coefficients
   E.g., x² + 3y = 2x + 1

MDPR THEOREM:
   Determining if Diophantine equation has integer solution
   is UNDECIDABLE

Significance:
   Resolved Hilbert's 10th Problem (1900)
```

**Be able to:**
- Give examples of Diophantine equations
- Understand why this matters

---

### 8. Rice's Theorem

**Memorize:**

```
SEMANTIC property:
   L is semantic if M, N compute same function
   ⟹ M ∈ L ⟺ N ∈ L
   (Property of WHAT function, not HOW)

RICE'S THEOREM:
   Every non-trivial semantic property is undecidable

Non-trivial = ∅ ⊂ L ⊂ {all TMs}
   (Not empty, not everything)
```

**Be able to:**
- Identify if a property is semantic
- Determine if it's non-trivial
- Apply Rice's Theorem

---

## Common Exam Question Types

### Type 1: Define and Explain

**Example Question:**
"Define a Turing Machine formally. What are the three components?"

**How to Answer:**
1. Give formal definition M = (Σ, Q, δ)
2. Explain each component
3. Give constraints (what must be included)
4. Give example

---

### Type 2: Trace Execution

**Example Question:**
"Given this TM and input '101', trace the first 3 steps"

**How to Answer:**
1. Write initial configuration I_x
2. Apply δ to get next configuration
3. Repeat for each step
4. Show intermediate states clearly

---

### Type 3: Computability Questions

**Example Question:**
"Is the palindrome problem computable? Decidable in polynomial time?"

**How to Answer:**
1. State definition of "computable"
2. Give algorithm/reason why yes or no
3. Analyze time complexity
4. Classify as polynomial/exponential

---

### Type 4: Undecidability Proofs

**Example Question:**
"Prove the halting problem is undecidable"

**How to Answer:**
1. State the halting problem formally
2. Assume contradiction (assume it's decidable)
3. Show paradox construction
4. Conclude undecidable

---

### Type 5: Apply Rice's Theorem

**Example Question:**
"Is the property 'machine halts on all inputs' decidable?"

**How to Answer:**
1. Check if semantic (does only function matter?)
2. Check if non-trivial (not empty/not everything?)
3. Apply Rice's Theorem
4. Conclude decidable/undecidable

---

## Practice Problems with Solutions

### Practice Problem 1

**Question:** Determine if the function is computable:

```
f(x) = { 1  if x has equal number of 0s and 1s
       { 0  otherwise
```

**Solution:**

```
Is f computable? YES ✓

Algorithm:
──────────

1. Count all 0s in string x
2. Count all 1s in string x
3. If count(0s) = count(1s):
     Output 1
   Else:
     Output 0

Why Computable:
───────────────
• Both counting operations are elementary
• Comparison is elementary
• We can implement this on a TM

Time Complexity:
────────────────
T(n) = 2n + constant = O(n)

Classification: POLYNOMIAL TIME ✓ (Linear)

Example:
────────
x = "1010"
Count 0s: 2
Count 1s: 2
Equal? YES → Output: 1 ✓

x = "111"
Count 0s: 0
Count 1s: 3
Equal? NO → Output: 0 ✓
```

---

### Practice Problem 2

**Question:** Apply Rice's Theorem

```
Is this language decidable?

L = {⟨M⟩ | M computes a total function}

(A total function is one defined on ALL inputs)
```

**Solution:**

```
Step 1: Is it semantic?
────────────────────

If M and N compute the same function,
does the property hold for both?

If M computes total function f,
and N also computes f,
then N also computes total function (same f!)

YES ✓ This is SEMANTIC

Step 2: Is it non-trivial?
───────────────────────────

Not ALL machines compute total functions?
→ YES, some compute partial functions (loop forever on some input)

Not NO machines compute total functions?
→ YES, many machines compute total functions

So: ∅ ⊂ L ⊂ {all TMs} ✓

Step 3: Apply Rice's Theorem
─────────────────────────────

L is semantic AND non-trivial

By Rice's Theorem:
→ L is UNDECIDABLE ✗

Conclusion:
───────────

Cannot determine if arbitrary program always terminates
This is intuitive: would solve halting problem!
```

---

### Practice Problem 3

**Question:** Time Complexity Analysis

```
Given these time functions, order them from fastest to slowest:

T₁(n) = n³ + 5n
T₂(n) = n log n + n²
T₃(n) = 2ⁿ
T₄(n) = 50
```

**Solution:**

```
Compare functions for large n:
──────────────────────────────

T₄(n) = 50
→ Constant, doesn't depend on n
→ Always 50 steps regardless of input
→ FASTEST

T₁(n) = n³ + 5n
For large n: dominated by n³
→ Cubic complexity O(n³)
→ Polynomial, reasonable speed

T₂(n) = n log n + n²
For large n: dominated by n²
→ Quadratic complexity O(n²)
→ Faster than cubic

T₃(n) = 2ⁿ
→ Exponential complexity O(2ⁿ)
→ SLOWEST (grows fastest with n)

Numerical check:
n = 10:  T₁ = 1050,   T₂ ≈ 133,  T₃ = 1024,   T₄ = 50
n = 20:  T₁ = 8100,   T₂ ≈ 406,  T₃ ≈ 1M,     T₄ = 50
n = 30:  T₁ ≈ 27000,  T₂ ≈ 944,  T₃ ≈ 1B,     T₄ = 50

Ordering (fastest to slowest):
T₄(n) < T₂(n) < T₁(n) < T₃(n)

Or: Constant < Polynomial < Exponential
```

---

## Important Definitions to Know

| Term | Definition |
|------|-----------|
| **Computable Function** | Function f where ∃ TM M: M(x)=f(x) for all x |
| **Decidable Language** | Language L where ∃ TM M: M(x)=1 if x∈L, M(x)=0 if x∉L |
| **Configuration** | Complete state of machine (state, tapes, head positions) |
| **Transition Function** | Function δ describing machine behavior at each step |
| **Universal TM** | TM U that can simulate any other TM |
| **Undecidable Problem** | Problem for which no TM can always give correct YES/NO answer |
| **Semantic Property** | Property of what function is computed (not how it's written) |
| **Halting Problem** | Problem: "Does machine M halt on input x?" (undecidable) |
| **Polynomial Time** | Time function T(n) = nᵏ for some constant k |
| **Exponential Time** | Time function T(n) = 2^(cn) for some constant c |

---

## Final Exam Preparation Checklist

Before the exam, make sure you can:

**Conceptual Understanding:**
- [ ] Explain why we need an abstract computational model
- [ ] Describe intuitively how a Turing Machine works
- [ ] Draw and interpret machine configurations
- [ ] Apply the transition function
- [ ] Distinguish computability from efficiency
- [ ] Understand uncomputability and paradoxes

**Technical Definitions:**
- [ ] Write formal definition M = (Σ, Q, δ)
- [ ] Give all constraints on Σ and Q
- [ ] Explain notation: T ⊢ configuration, M(x) = y
- [ ] Define computable function formally
- [ ] Define decidable language formally

**Theorems:**
- [ ] State UTM theorem and understand implications
- [ ] Sketch proof of uncomputability
- [ ] Sketch proof of halting problem undecidability
- [ ] State Rice's Theorem
- [ ] State MDPR Theorem
- [ ] Understand robustness

**Problem Solving:**
- [ ] Trace TM execution on small examples
- [ ] Determine if function is computable
- [ ] Analyze time complexity of algorithms
- [ ] Apply Rice's Theorem to decide decidability
- [ ] Create simple TM descriptions

**Examples:**
- [ ] Palindrome checker algorithm
- [ ] Parity computation
- [ ] Uncomputability function f_uc
- [ ] Halting problem
- [ ] Various undecidable problems

---

## Good Luck! 🎓

**Key Takeaway:** Computability and complexity are about fundamental limits. Not about being clever—some problems are mathematically impossible to solve with ANY algorithm. Understanding this changes how we think about computers!

