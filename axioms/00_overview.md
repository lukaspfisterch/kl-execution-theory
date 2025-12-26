# Axiom Overview

KL Execution Theory describes deterministic execution using a small set of
core concepts:

- **S** – States of a system  
- **Δ** – Atomic state transitions  
- **V** – Ordered behaviour as a sequence of Deltas  
- **t** – Logical time as index into behaviour  
- **G** – Governance derived from behaviour  
- **L** – Boundaries derived from behaviour  

This document introduces the five core axioms. Each axiom has its own
dedicated file with a precise definition and further discussion.

## Failure Modes (Index)
- FM-1 Non-deterministic Delta (same state -> different output).
- FM-2 Behaviour incomplete (executed step missing from V).
- FM-3 Logical time ambiguous or not index-based.
- FM-4 Trace not reproducible given same s0 and V.
- FM-5 Governance decision not derivable from V (uses external state).
- FM-6 Boundary violation not deterministically demonstrable from V.
- FM-7 Observational time treated as semantic ordering (t_wall/t_perf used as t_index).
- FM-8 Governance or boundaries execute or modify behaviour.

Each axiom file includes a short FM mapping back to this list.

---

## 1. States and Deltas

A system has a set of possible states **S**.

Execution proceeds through **atomic and indivisible state transitions**:

> Δ: S → S

Each Δ consumes one state and produces the next state.  
Δ is not decomposed further at the chosen level of abstraction.

---

## 2. Behaviour (V)

**Behaviour V** is the ordered sequence of all executed Deltas:

> V = { Δ₀, Δ₁, …, Δₙ }

There is no parallelism at the level of the theory.  
Only one Delta is applied at a time, and behaviour is the total sequence
of these transitions.

---

## 3. Logical Time (t)

Logical time **t** is derived from behaviour.

Time is the index of a Delta in the sequence V:

> t ∈ {0, 1, …, n}  
> Δₜ is the Delta at logical time t

There is no external clock.  
Time exists only because behaviour exists and can be indexed.

---

## 4. Governance (G)

Governance **G** is a function over behaviour:

> G = f(V)

G does not execute or modify behaviour.  
It classifies behaviour according to some policy view, for example:

- valid vs invalid  
- compliant vs non-compliant  
- acceptable vs unacceptable

All governance is derived from the executed behaviour V.

---

## 5. Boundaries (L)

Boundaries **L** describe the constraint geometry induced by behaviour:

> L = g(V)

L captures which regions of state and behaviour space are:

- allowed or disallowed  
- safe or unsafe  
- inside or outside design constraints

Like G, boundaries do not execute or control behaviour.  
They are derived descriptions of which regions are visited, avoided or
forbidden under a given perspective.

---

## 6. Derived Execution View

The theory does not introduce a separate axiom for "execution".

Execution is the **emergent view** of:

- states S  
- atomic transitions Δ  
- behaviour V as the ordered sequence of Deltas  
- logical time t as the index into V  
- governance G and boundaries L as derived evaluators

This keeps the axiom set minimal while still supporting rich
interpretations in concrete domains (software, finance, biology,
governance systems and more).
