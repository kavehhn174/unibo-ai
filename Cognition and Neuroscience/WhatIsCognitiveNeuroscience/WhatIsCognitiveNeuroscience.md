# What Is Cognitive Neuroscience? — Bridging the Gap Between Brain and Mind

**Course:** Cognition and Neuroscience — University of Bologna, A.Y. 2025/2026  
**Instructor:** Francesca Starita ([francesca.starita2@unibo.it](mailto:francesca.starita2@unibo.it))

---

## 📌 Overview

This lecture introduces the field of **cognitive neuroscience** — an interdisciplinary science at the intersection of brain structure (neuroscience) and mental function (cognition). It sets up the entire course by asking *why* studying the brain is relevant to AI and provides concrete evidence (from brain-damaged patients) that brain structure and cognitive function are inseparable.

---

## 🎯 Learning Objectives

By the end of this lecture you should be able to:

1. Define **neuroscience**, **cognition**, and **cognitive neuroscience**.
2. Explain the relationship between neuroscience and AI.
3. Discuss arguments **for and against** using the human brain as a model for AI.
4. Describe the different **levels of brain emulation** in AI.
5. Explain how brain **structure and function** are intimately related.
6. Explain how cognitive neuroscience bridges the gap between brain structure and the mind.

---

## 🔑 Key Concepts

### Neuroscience
> **The study of how the nervous system is organized and functions.**

Neuroscience is a **multidisciplinary** science, drawing from:
- Physiology
- Anatomy
- Molecular biology
- Developmental biology
- Cytology
- Computer science
- Mathematical modeling

**Levels of investigation:**

| Level | What it Studies |
|---|---|
| **Molecular** | Molecular neuroanatomy; mechanisms of molecular signaling in the nervous system |
| **Cellular** | Neurons at the cellular level — morphology and physiological properties |
| **Neural circuits & systems** | How circuits are formed and function to generate behaviors (sensory perception, motor reflexes, etc.) |

---

### Cognition
> **The range of mental processes relating to the acquisition, storage, manipulation, and retrieval of information.**

Cognition includes multiple processes:

| Process | Description |
|---|---|
| **Perception** | Takes in information from the environment through the senses |
| **Attention** | Focuses on a specific stimulus in the environment |
| **Learning** | Manipulates new information and integrates it with prior knowledge |
| **Memory** | Encodes, stores, and retrieves information; critical for learning |
| **Action** | Uses perceived information to interact with the environment |
| **Language** | Understanding and expressing thoughts through spoken/written words |
| **Thought / Higher Reasoning** | Decision-making and problem-solving |

---

### Cognitive Neuroscience
> **The interdisciplinary field that studies the neural basis of cognition, seeking to understand how the nervous system gives rise to mental processes.**

It sits at the intersection of:
- **Neuroscience** (structure)
- **Cognition** (function)

> 💡 **TA Note:** Think of it as the "translation layer" — it asks not just *what* the brain looks like or *what* cognitive tasks humans can do, but *how one gives rise to the other*. This is exactly what AI researchers need to understand if they want to build machines that genuinely replicate intelligent behavior.

---

## 🧠 Why Is This Course in an AI Degree?

### Arguments FOR using the brain as a model for AI

**Conceptual / Theoretical:**
- The human brain is the only existing **proof that general intelligence is possible** at all.
- Studying animal cognition can provide a window into different aspects of higher-level general intelligence.

**Technical / Mechanistic:**
- **Functional similarities** exist between biological brains and artificial computations (e.g., the all-or-none firing of neurons ↔ binary computation).
- Neuroscience can **inspire new types of algorithms and architectures**, independent of or complementary to purely mathematical/logic-based methods.
- Neuroscience can **validate existing algorithms**: if a known algorithm is found to be implemented in the brain, that gives strong support for its plausibility as part of a general intelligence system.

> 💡 **TA Note:** The relationship is bidirectional — AI also provides insights into how the brain works. This is biomimicry in reverse.

---

### Arguments AGAINST using the brain as a model for AI

**Conceptual / Theoretical:**
- Modeling brains and computers on each other may **prevent deeper, novel insights**.
- From an engineering perspective, *what works is ultimately all that matters* — biological plausibility need not be enforced.
- We still do **not know the detailed circuitry** of any brain region well enough to reproduce it.

**Technical / Mechanistic:**
- Unlike hardware/software, **mind and brain are not distinct entities** — the software *emerges* from the hardware. This cannot be cleanly separated.
- Neurons transmit information not just through electrical signals but through **subtle biochemical changes**.
- Neuronal communication operates in **cycles** (recurrent), not linear chains — requiring recurrent components in neural networks (cf. Kar et al., 2019, *Nature Neuroscience*).
- The brain generalizes from **very few examples**; ML models require vast datasets — a fundamental asymmetry.

> ⚠️ **Exam-Relevant** — Be prepared to list and explain 2–3 arguments on *each* side. The exam may ask you to critically analyze the brain-as-model-for-AI debate.

---

## 🏗️ Levels of Brain Emulation in AI

| Approach | Focus | Example |
|---|---|---|
| **Structure-focused** | Closely mimic or reverse-engineer specific neural circuits | **Blue Brain Project** (Markram, 2006) — biologically detailed digital reconstruction of the mouse brain |
| **Function-focused** | Mimic computational/algorithmic levels of neural systems | **DeepMind** (Hassabis, Legg, 2010) — systems neuroscience-level understanding; algorithms, architectures, representations |

> ⚠️ **Exam-Relevant** — Know the difference between structure-level and function-level emulation and be able to give an example of each.

---

## 🏛️ Brain Structure & Function Are Intimately Related

> **Cognitive functions emerge from the structure of the nervous system.**

### The Brain: 6 Subdivisions

1. **Medulla** — Brainstem
2. **Pons** — Brainstem
3. **Midbrain** — Brainstem
4. **Cerebellum**
5. **Diencephalon** (thalamus & hypothalamus)
6. **Telencephalon** (cerebral hemispheres) — the largest part of the human brain

### The Telencephalon
Consists of:
- **Cerebral cortex** — grey matter (neuronal cell bodies)
- **White matter** — axons and glial cells
- Three deep structures: **Basal ganglia**, **Amygdala**, **Hippocampus**

### The Cerebral Cortex
- Two symmetrical hemispheres connected by the **corpus callosum**
- Divided into **4 lobes** distinguished by prominent sulci (grooves)
- Cognitive systems are organized as **hierarchical networks** spanning multiple lobes (primary → secondary → tertiary areas)

---

## 🧩 Evidence: Structure Determines Function

### Method: Brain Lesion Studies
Focal brain damage causes **specific cognitive and behavioral deficits**.

**Types of lesions:**
- Naturally occurring (tumor, stroke, degenerative disease)
- Surgically induced (to treat epilepsy — the **Montreal procedure**)
- Experimentally caused (animals only)

> **Key principle:** Brain lesions provide **causal evidence** on the relation between structure and function. They identify which region is *necessary* for a given behavior.

---

### Case Study: Split-Brain & Hemispheric Specialization

- The two hemispheres are connected via the **corpus callosum** (largest white matter structure in the brain).
- Sensory and motor activities on one side of the body are mediated by the **contralateral** (opposite) hemisphere.
- Most CNS pathways **cross over** to the contralateral side.
- In healthy individuals, information from both hemispheres is **integrated** via the corpus callosum.
- In **split-brain patients** (corpus callosum severed), this integration is impaired, **revealing hemispheric specialization**.

> 💡 **TA Note:** This is a profound insight — by *breaking* the connection, researchers could see what *each side* does independently. It's a natural experiment revealing modularity of brain function.

---

### Language and the Left Hemisphere

> ⚠️ **Exam-Relevant** — The double dissociation between Broca's and Wernicke's aphasia is a classic exam topic.

**Broca's Area** (left inferior frontal lobe):
- Discovered by **Paul Broca (1861)** via patient "Tan"
- Damage → **Expressive (Broca's) aphasia**:
  - Impaired language *production* (non-fluent)
  - Preserved language *comprehension*
  - Patient is aware of their deficits

**Wernicke's Area** (left superior temporal gyrus):
- Discovered by **Carl Wernicke (1876)**
- Damage → **Receptive (Wernicke's) aphasia**:
  - Impaired language *comprehension*
  - Fluent but *nonsensical* speech production
  - Patient often unaware of errors

**Double Dissociation:**

| Patient Group | Task X (Production) | Task Y (Comprehension) |
|---|---|---|
| Broca's lesion | ❌ Impaired | ✅ Preserved |
| Wernicke's lesion | ✅ Preserved (but jumbled) | ❌ Impaired |

> 💡 **TA Note:** A **double dissociation** is the gold standard for proving two cognitive functions are implemented by *distinct* neural substrates. If damaging area A impairs task X but not Y, and damaging area B impairs Y but not X — the two tasks are neurally separable.

---

### The Montreal Procedure & Penfield's Maps

- **Wilder Penfield (1891–1976)**: Treated epilepsy by surgically destroying seizure-producing neurons.
- He electrically stimulated brain areas during awake surgery, observing responses to map the **sensory and motor cortices** (Penfield & Jasper, 1954).
- **Donald Hebb (1904–1985)**: Proposed that learning has a biological basis — *"cells that fire together, wire together."* Hebb's theory directly inspired **artificial neural networks**.

> ⚠️ **Exam-Relevant** — Hebb's rule is foundational to how ANNs learn. Know the basic principle.

---

### Case Study: Patient H.M. & The Hippocampus

- **H.M.** had his **medial temporal lobes** (including the hippocampus) surgically removed to treat severe epilepsy.
- **Brenda Milner** studied him and found:
  - ❌ Could **not** form new long-term declarative memories (**anterograde amnesia**)
  - ✅ Normal **short-term memory**
  - ✅ Normal **procedural memory** (could learn motor skills, but couldn't *remember* learning them)

**Conclusion:**
- Memory is **not** a unitary system.
- The **medial temporal lobe** (hippocampus) is necessary for forming new **long-term declarative memories** and transferring short-term to long-term.
- It is **not** necessary for procedural/motor learning.

> 💡 **TA Note:** This was revolutionary — before H.M., it was assumed memory was inseparable from general intelligence. H.M. showed they are dissociable, and that different memory types have *different anatomical substrates*.

---

## 📐 Memory Systems

### Definition
> **Memory** = The process of encoding, storage, and retrieval of information.
> **Learning** = Acquiring new information; memory is the outcome of learning.

### Basic Steps of Memory Processing

1. **Encoding** — Processing incoming information to create memory traces:
   - *Acquisition*: Sensory stimuli enter short-term memory.
   - *Consolidation*: Stabilization over time (days to years) → long-term memory.
2. **Storage** — "Permanently" recording the information.
3. **Retrieval** — Accessing stored information to create a conscious representation or execute a learned behavior.

### Neural Plasticity
> Neural connections can be **modified by experience and learning**.

- **Short-term changes**: Functional/physiological changes (seconds to hours) — increase or decrease effectiveness of existing synaptic connections (*Hebbian plasticity*).
- **Long-term changes**: Structural changes (days) — anatomical alterations including pruning or growth of synapses.

---

### Case Study: Phineas Gage & the vmPFC

- **Phineas Gage (1848)**: A railroad worker whose left frontal lobe was destroyed by an iron rod — he survived but his *personality changed dramatically*.
- Implicated the **ventromedial prefrontal cortex (vmPFC)** in **cognitive/affective control** and decision-making.
- The **orbitofrontal cortex** (most ventral part of vmPFC) lies above the bony orbits of the eyes.

### Case Study: Hemispatial Neglect & the Parietal Lobe

- **Hemispatial neglect**: A disorder of attention; patients ignore one side of the world.
- Most common after lesion to the **right parietal lobe** → left-sided neglect.
- Symptoms: Poor spatial awareness, impaired attention to environment, difficulty interpreting visual scenes as a whole.

---

## ⚠️ Exam-Relevant Topics

| Topic | What to Know |
|---|---|
| **Brain as AI model** | 2–3 arguments for AND against; reference the Nature 2012 paper by Brooks, Hassabis et al. |
| **Levels of brain emulation** | Structure-level (Blue Brain) vs. Function-level (DeepMind) |
| **Double dissociation** | Definition + example (Broca's vs. Wernicke's aphasia) |
| **Hebb's rule** | "Cells that fire together, wire together" — basis of ANN learning |
| **H.M. case study** | What it proves about multiple memory systems and the hippocampus |
| **Broca's vs. Wernicke's aphasia** | Symptoms, lesion location, type of deficit |
| **Hemispheric specialization** | Left hemisphere dominant for language (96% of people); split-brain evidence |
| **Neural plasticity** | Short-term vs. long-term; Hebbian plasticity |

---

## 📝 Summary

- **Neuroscience** studies the structure and function of the nervous system at multiple levels (molecular → cellular → systems).
- **Cognition** is the full range of mental processes for acquiring, storing, manipulating, and retrieving information.
- **Cognitive neuroscience** bridges the two: it asks how brain structure gives rise to cognitive function.
- The brain is a useful but **imperfect** model for AI — it proves general intelligence is possible, but differs from computers in fundamental ways (no hardware/software separation, biochemical signaling, recurrent communication, few-shot generalization).
- Two levels of AI brain emulation exist: **structure-focused** (Blue Brain) and **function-focused** (DeepMind).
- Brain lesion studies provide **causal evidence** linking structure to function — key examples include split-brain patients, Broca's and Wernicke's aphasia, Patient H.M., Phineas Gage, and hemispatial neglect.
- **Hebb's rule** ("cells that fire together, wire together") directly inspired artificial neural network design.
- Memory is **not unitary**: H.M. demonstrated that declarative and procedural memory systems are neurally dissociable.

---

## 📚 Recommended Readings

- Brooks R, Hassabis D, Bray D, Shashua A. *Turing centenary: Is the brain a good model for machine intelligence?* Nature. 2012 Feb 22;482(7386):462-3. doi: [10.1038/482462a](https://doi.org/10.1038/482462a)
- Gazzaniga, Ivry, Magnum. *Cognitive Neuroscience: The Biology of the Mind*
  - Chapter 4 – Hemispheric Specialization
  - Chapter 9 – Memory

---

## 🔁 Revision Questions

1. **Analyze the relationship between neuroscience and AI.** To what extent is the brain a good model for machine intelligence, and what are the potential benefits and limitations?
2. **Structure and function are intimately related in the nervous system.** Choose one example from cognitive neuroscience to illustrate that cognition directly emerges from nervous system structure.
