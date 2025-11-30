# Axiom 1: Delta (Δ)

Delta (Δ) is the fundamental unit of execution.  
It represents a single, indivisible state transition within a system.

This axiom defines the smallest meaningful step in the evolution of a system.

---

## Definition

Let **S** be the set of all possible system states.

A Delta is a total function:

**Δ : S → S**

This function takes a state and produces a new state.

There is no intermediate state, no decomposition and no observable  
substructure inside Δ.  
It is atomic by definition.

---

## Properties

### 1. Indivisibility
A Delta cannot be split into smaller execution steps.

If Δ could be decomposed, the decomposition would itself define  
the actual Deltas, and Δ would not be atomic.

---

### 2. Determinism
Given the same input state, a Delta always produces the same output state.

Formally:

If **s₁ = s₂**, then **Δ(s₁) = Δ(s₂)**.

This ensures reproducibility, auditability and consistent behaviour  
across implementations.

---

### 3. No Side Channels
A Delta does not observe or rely on external time, randomness,  
network state or non-deterministic sources.

Any such dependency would violate determinism and atomicity.

---

### 4. Domain Neutrality
Δ does not encode domain semantics.  
It merely describes an atomic transformation.

Domain meaning emerges only when Δ is interpreted within a specific context.

---

## Interpretation

A Delta represents:

- a minimal change  
- an isolated effect  
- an atomic operation  
- a state update  
- the smallest executable element in a system

All complex execution behaviour is constructed from sequences of Δ.

---

## Examples (Domain-Agnostic)

### Mathematical Example
A pure function **Δ(x) = x + 1** applied to integer state.

### Financial Example
Applying a single limit order to an order book state  
(when defined as a pure transformation).

### Biological Example
A stoichiometric reaction step  
(e.g., glucose + atp → glucose_6_phosphate + adp)  
in a discrete, deterministic interpretation.

### Computational Example
A single instruction in a deterministic virtual machine.

These examples illustrate interpretation, not semantics.

---

## Non-Examples

The following are *not* valid Deltas:

- Random number generation  
- Time-dependent operations  
- Nondeterministic IO  
- Parallel behaviour  
- Partially applied operations  
- Any step whose outcome depends on external mutable context

These violate determinism or atomicity.

---

## Relation to Behaviour

Delta is the building block of Axiom 2.  
Sequences of Δ define all observable behaviour of a system:

**V = { Δ₀, Δ₁, …, Δₙ }**

No additional structure is required to describe execution.

---

## Summary

Delta serves as the indivisible kernel of execution:

- atomic  
- deterministic  
- pure  
- domain-neutral  
- free of side channels  
- the fundamental unit from which behaviour is constructed

All higher-level structures in this theory derive from sequences  
of these indivisible transformations.
