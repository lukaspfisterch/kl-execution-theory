# Axiom 2: Behaviour (V)

Behaviour is the ordered sequence of all executed Deltas.  
It is the complete record of how a system evolves over time.

This axiom establishes that execution is fundamentally sequential and that  
the observable behaviour of a system is entirely captured by the order of  
its atomic transitions.

---

## Definition

Given a set of states **S** and atomic transitions **Δ : S → S**,  
behaviour **V** is defined as:

**V = { Δ₀, Δ₁, Δ₂, …, Δₙ }**

This sequence represents every executed Delta in the exact order of execution.

V is a *total order*.  
Parallelism does not exist at the level of this abstraction.

---

## Properties

### 1. Total Order
All execution appears as a single sequence.

There is no branching, no concurrency and no ambiguity in ordering.  
If behaviour is observable, it must be totally ordered.

Formally:

For all **i, j** in V,  
either **i < j** or **j < i** or **i = j**.

---

### 2. Completeness
V captures the entire execution history.

Nothing outside V influences the evolution of the system.  
All state transitions are contained within this sequence.

---

### 3. Sequential Determinism
Since each Δ is deterministic (Axiom 1),  
the entire sequence V is deterministic.

Given initial state **s₀** and sequence **V**,  
the resulting state is uniquely determined.

Formally:

**sₙ = Δₙ( Δₙ₋₁( ... Δ₀(s₀) ... ))**

There are no alternative interpretations.

---

### 4. No Implicit Semantics
V does not prescribe meaning to the sequence.  
It only provides the structure.

Interpretation of the sequence happens outside the axiom,  
for example within a domain or an evaluation layer.

---

## Interpretation

Behaviour is the **only** observable manifestation of execution.

It represents:

- the execution trace  
- the causal ordering of transitions  
- the replayable history of the system  
- the source of all derived evaluations (governance, boundaries)

V is the canonical reference for all derived analysis.

---

## Examples (Domain-Agnostic)

### Computational Example
A sequence of instructions executed in a deterministic VM.

### Financial Example
A sequence of orderbook Deltas (new order, fill, cancel).

### Biological Example
A sequence of deterministic reaction steps in a discrete chain.

### AI Execution Example
A sequence of validated steps in a controlled inference pipeline.

In all cases, the sequence is the mechanical representation of behaviour.

---

## Non-Examples

These are not valid forms of behaviour under this axiom:

- unordered events  
- concurrent or parallel execution graphs  
- probabilistic event sets  
- states without transitions  
- transitions without a defined ordering

Such structures violate total ordering or determinism.

---

## Relation to Time

Behaviour defines logical time.  
There is no external temporal axis.  
Time is simply:

**t = index(V)**

(This is covered in Axiom 3.)

---

## Summary

Behaviour (V) is the ordered, deterministic, complete sequence of all  
executed Deltas. It defines how a system evolves and serves as the  
foundation for:

- logical time  
- governance evaluation  
- boundary derivation  
- auditability  
- replayability

It is the core observable structure of deterministic execution.
