# Fundamentals of AI and Knowledge Representation – Module 2

## A student-friendly introduction

This text rewrites the introductory slides of **Module 2** from the course *Fundamentals of AI and KR* into a continuous, self-study format. It is designed for students who want to understand the purpose, structure, resources, and expectations of the module without depending on the slide format.[file:1]

## What this module is about

This module is centered on one of the core areas of artificial intelligence: **knowledge representation and reasoning**. In AI, it is not enough to store information like a database does. An intelligent system must also be able to organize what it knows, express that knowledge in a formal way, and use it to draw conclusions, answer questions, or support decisions.[file:1]

The slides make an important point from the beginning: there is not just one way to represent knowledge. Over time, researchers have developed many paradigms for this purpose. Some approaches are based on formal logics, while others rely on rules and rule-based systems. This module introduces students to that landscape and prepares them to see how different techniques are suited to different kinds of problems.[file:1]

### Author note

A useful way to think about this field is to ask three simple questions:

1. What do we want the machine to know?
2. How do we encode that knowledge?
3. Once encoded, how can the machine reason with it?

These three questions are the real backbone of the module, and they reappear throughout the lessons in different forms.[file:1]

## The main objective

The explicit objective of the module is to introduce students to the general topic of **KR&R techniques**, that is, Knowledge Representation and Reasoning techniques.[file:1]

The course frames this objective through three progressive questions.

First, it asks what knowledge actually is and what exactly should be represented. This is more subtle than it sounds. Knowledge may include facts, categories, events, time, uncertainty, processes, or relationships among objects.[file:1]

Second, it asks how knowledge should be represented. This leads to formal languages and models such as propositional logic, first-order logic, ontologies, semantic networks, description logics, rules, and process models.[file:1]

Third, it asks how reasoning can be performed on represented knowledge. In other words, once knowledge has been encoded, how can a system infer new facts, answer queries, deal with time, or reason under uncertainty?[file:1]

### Author note

These three questions also suggest the learning progression of the module:

- from understanding knowledge,
- to modeling knowledge,
- to operating on knowledge.

That progression is pedagogically important because students should not learn a formalism as if it were just syntax. Each formalism is introduced because it solves a representation or reasoning problem.[file:1]

## Reference books

The introductory slides list several books that support the module.[file:1]

The first and most general reference is *Artificial Intelligence: A Modern Approach* by Russell and Norvig, 4th edition. The slides explain that, when a topic is discussed in class, they will point to the corresponding chapter of the book.[file:1]

Other important references are *Knowledge Representation and Reasoning* by Brachman and Levesque, *Process Mining* by van der Aalst, and *Foundations of Probabilistic Logic Programming* by Riguzzi, second edition.[file:1]

### Author note

These references already reveal the breadth of the module. It is not limited to symbolic logic in a narrow sense. It also includes semantic technologies, probabilistic reasoning, and process-oriented modeling. That means students should expect a module that connects theory with practical representation techniques used in different AI-related contexts.[file:1]

## Structure of the module

The module is divided into seven parts, numbered from Part 0 to Part 6.[file:1]

| Part | Theme | What it means for the student |
|---|---|---|
| Part 0 | A common language | Establishes the basic logical vocabulary shared by the rest of the course.[file:1] |
| Part 1 | First-Order Logic for representing knowledge | Introduces a useful fragment of logic and operational reasoning through Prolog.[file:1] |
| Part 2 | Knowledge about facts and categories | Focuses on ontologies, semantic structures, and conceptual organization.[file:1] |
| Part 3 | Knowledge about temporal information | Studies how to represent time, events, and temporal relationships.[file:1] |
| Part 4 | Probabilistic knowledge and reasoning | Adds uncertainty to logical representation and inference.[file:1] |
| Part 5 | Forward vs. backward reasoning | Explores rule execution strategies and rule-engine technology.[file:1] |
| Part 6 | Representing processes | Examines process models, workflows, and decision representation.[file:1] |

This organization shows that the module moves from foundations to more specialized applications. The early parts build the formal language needed for the later parts, while the final sections show how representation and reasoning apply to realistic domains such as temporal systems, uncertain knowledge, and business processes.[file:1]

## Part 0: A common language

Part 0 asks a fundamental question: can everyone in the course agree on a common language? The answer proposed by the module is to begin with formal logic, especially the basic concepts that make reasoning precise and shared.[file:1]

Lesson 1 introduces propositional logic, first-order logic, interpretation, models, logical consequence, and resolution.[file:1]

### Why this matters

Students often want to move quickly to programming tools, but this opening lesson has a deeper purpose. If two people use the same words but mean different things, reasoning becomes unreliable. A common logical language provides exact meaning, clear assumptions, and a framework for deciding whether a conclusion follows from given premises.[file:1]

### Author note

This part is like agreeing on grammar before writing essays. Without a shared formal vocabulary, later topics such as ontologies, temporal reasoning, or probabilistic logic would feel disconnected. With it, the rest of the module becomes much easier to unify conceptually.[file:1]

## Part 1: First-Order Logic and Prolog

Part 1 is devoted to a fragment of First-Order Logic that can actually be reasoned upon in practice.[file:1]

Lesson 2 introduces Prolog, terminology, SLD resolution, arithmetic, iteration and recursion, lists, cut, and negation. Lesson 3 moves to meta-predicates, and Lesson 4 to meta-interpreters.[file:1]

This part is important because it bridges logical representation and executable reasoning. Prolog is not studied only as a programming language, but as a way to operationalize logic. Students begin to see how formal statements can be turned into computations that answer queries and derive consequences.[file:1]

### Author note

This is usually one of the most intellectually rich sections of the course. At first, Prolog may look unusual compared with imperative programming. But once its logic-based style becomes familiar, it reveals how reasoning itself can be programmed. Meta-predicates and meta-interpreters push that idea further by showing how one can reason about reasoning.[file:1]

## Part 2: Facts, categories, and semantic structures

Part 2 studies how to represent knowledge about facts, objects, and categories.[file:1]

Lesson 5 includes upper ontologies, objects and categories, reification, disjointness, exhaustive decomposition, partition, physical composition, measures, and the distinction between things and stuff, as well as intrinsic and extrinsic properties. Lesson 6 covers semantic networks. Lesson 7 introduces description logics. Lesson 8 presents Protégé. Lesson 9 moves to the Semantic Web and Knowledge Graphs.[file:1]

This part is about conceptual modeling. It asks not only whether something is true, but also how entities are classified, how concepts are related, and how structured knowledge can be made explicit and reusable. The move from ontologies to semantic networks and description logics then naturally leads to practical tools and web-scale knowledge representation.[file:1]

### Author note

A student can think of this part as moving from isolated facts to organized worlds. A single fact such as “Socrates is a human” is useful, but an ontology tells us how “human,” “animal,” “person,” “organism,” and related concepts fit together. That larger structure is what makes knowledge interoperable and machine-usable in a deeper sense.[file:1]

## Part 3: Temporal knowledge

Part 3 focuses on knowledge that changes over time or depends on temporal relationships.[file:1]

Lesson 10 introduces Event Calculus and Allen's Temporal Logic. Lesson 11 covers modal logics and Linear-Time Temporal Logic (LTL). There is also an exercise on Event Calculus in Prolog.[file:1]

This section recognizes that many real situations cannot be represented adequately without time. Events happen, states begin and end, actions have consequences, and temporal order matters. A system that knows facts but cannot distinguish between “before,” “during,” and “after” remains too limited for many practical tasks.[file:1]

### Author note

Temporal reasoning is one of the places where abstract logic becomes very concrete. Schedules, workflows, monitoring systems, and planning problems all depend on it. If earlier parts teach students how to describe a world, this part teaches how to describe a world that evolves.[file:1]

## Part 4: Probabilistic reasoning

Part 4 addresses probabilistic knowledge and reasoning.[file:1]

Lesson 12 introduces Probabilistic Logic Programming, and the case study concerns the assessment of the fall risk of an elder.[file:1]

This part extends the earlier logical framework by acknowledging uncertainty. In real life, systems often do not know whether a fact is simply true or false. Instead, they may have incomplete evidence, noisy observations, or uncertain causal relationships. Probabilistic logic combines structured symbolic reasoning with degrees of uncertainty.[file:1]

### Author note

This is an important conceptual shift. Classical logic is excellent when knowledge is crisp and complete, but many applications in health, safety, or decision support require reasoning under uncertainty. The case study on fall risk helps ground that idea in a realistic and socially relevant scenario.[file:1]

## Part 5: Forward and backward reasoning

Part 5 is about the difference between forward and backward reasoning.[file:1]

Lesson 13 introduces forward reasoning through Rete and Drools. The module also presents a case study on complex event processing (CEP), combining temporal reasoning and forward reasoning through the Habitat project example.[file:1]

This part highlights an important operational question: should a system start from known facts and push forward toward consequences, or start from a goal and work backward to prove it? Students have already seen goal-directed reasoning in logic programming; here they encounter production-rule systems that react to available facts and evolving events.[file:1]

### Author note

This comparison is especially valuable because it shows that reasoning is not only about correctness, but also about strategy. Two systems may encode similar knowledge and still behave very differently depending on whether they reason backward from questions or forward from incoming facts.[file:1]

## Part 6: Representing processes

Part 6 studies how to represent processes.[file:1]

Lesson 14 introduces BPM, Lesson 15 covers Workflow Nets and BPMN, Lesson 16 discusses declarative approaches, Lesson 17 covers Process Discovery, and Lesson 18 addresses Representing Decisions.[file:1]

This final part broadens the notion of knowledge once again. Knowledge is not only about static facts or taxonomies. It can also describe how activities unfold, how tasks depend on one another, how decisions are embedded in processes, and how actual behavior can be discovered from recorded traces.[file:1]

### Author note

This is where the module becomes strongly process-oriented. It links AI and KR with organizational workflows and operational behavior. Students should notice that by the end of the module, “knowledge” has expanded from logical statements to dynamic, structured, and decision-rich systems.[file:1]

## Learning resources and software

The module identifies several learning resources: the textbook, the course slides, and scientific papers mentioned in each topic lesson.[file:1]

It also lists the software that students may encounter: SWI Prolog, Drools together with Java, Python with the Google API for some experiments, and DISCO for process discovery.[file:1]

### Author note

This list is a practical map of the course. SWI Prolog supports the logic-programming parts, Drools supports rule-based reasoning, Python is useful for experimentation, and DISCO connects the module to process mining practice. Students should therefore expect both conceptual study and hands-on exposure to tools.[file:1]

## Exam information

The slides state that four exam dates will be available: 13 January 2026, 29 February 2026, 23 June 2026, and 7 September 2026, all marked as “to be confirmed.” They also mention a special date in December 2025 reserved for Erasmus students only. Booking on AlmaEsami is mandatory.[file:1]

The exam is described as a written test on paper. It contains four exercises or questions, usually two exercises about Prolog or meta-interpreters and two open questions. The duration is one hour. Grades are given on a 21-point scale, where 20 is the maximum ordinary score and one extra point may be awarded for exceptional essays.[file:1]

The rules are strict: no material may be consulted during the exam, and consultation with colleagues is not allowed. If a student does not accept the grade and retries later, submitting the new exam causes the previous grade to be lost.[file:1]

Past exam examples can be found on virtuale.unibo.it according to the slides.[file:1]

### Author note

The exam structure suggests a balanced expectation. Students are not being evaluated only on theory or only on technical syntax. They are expected to combine operational competence in Prolog-related topics with broader conceptual understanding of KR&R themes.[file:1]

## Contact information

For communication, the slides provide the instructor's email address, mention Microsoft Teams for chat or calls, and list the office phone number.[file:1]

From a student's perspective, this is a reminder that the module should not be approached passively. Since the topics span logic, semantics, uncertainty, and processes, asking questions early is likely to be much more effective than waiting until exam preparation begins.[file:1]

## Final reading perspective

Taken as a whole, this introductory unit presents Module 2 as a guided journey through the major ways AI systems can represent and reason about knowledge.[file:1]

It begins with shared logical foundations, moves through executable logic and structured conceptual knowledge, then extends toward time, uncertainty, rule-driven inference, and processes.[file:1]

A student reading this introduction should come away with a clear expectation: the module is not about memorizing isolated definitions, but about learning a family of formalisms and tools that answer a common challenge—how to make knowledge explicit, structured, and usable for reasoning.[file:1]
