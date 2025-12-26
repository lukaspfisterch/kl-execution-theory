# Axiom 3: Time (t)

Time in this theory is not an external, physical quantity.  
It is a logical construct derived from behaviour.

Time exists only as an index into the sequence of executed Deltas.

---

## Definition

Given behaviour

**V = { Δ₀, Δ₁, Δ₂, …, Δₙ }**

logical time **t** is defined as:

**t ∈ { 0, 1, 2, …, n }**  
**t = index(V)**

Each position in V corresponds to one logical time step.

There is no additional temporal dimension.

---

## Minimal Contract (Implementation)

Inputs:
- V (ordered behaviour sequence)

Outputs:
- t_index in {0..n} and mapping t_index -> Delta in V

Invariants:
- Derived only from V
- Discrete and order-preserving
- No external clock dependence

Non-Goals:
- Wall clock time, durations, calendars

---

## Properties

### 1. Derived, Not Fundamental
Time is not an independent axis.  
It is derived from execution.

If there is no behaviour, there is no time.  
If behaviour stops, time stops.

---

### 2. Discrete
Time is discrete and indexed by integers.

There are no fractions, continuous intervals or external clocks.  
Only the sequence position matters.

---

### 3. Order Preserving
Logical time preserves the order of behaviour.

If **i < j**, then Δᵢ happens before Δⱼ.  
This is a direct consequence of Axiom 2 (Behaviour as total order).

---

### 4. Domain Neutral
Time does not encode:

- physical duration  
- latency  
- wall clock time  
- calendar dates  
- real world clocks

Any such mapping is an external interpretation, not part of the axiom.

---

## Observational Time Projections

The following are optional projections captured during execution:
- t_wall: wall clock timestamps; ordering-unsafe due to clock adjustments
- t_perf: monotonic duration; duration-safe but not semantic ordering
- t_index: semantic ordering within V and remains authoritative

---

## Interpretation

Logical time represents:

- the position of a Delta in the execution history  
- the step count of the system  
- the progress of behaviour in purely structural terms

All references to "before" or "after" in this theory are statements  
about the index in V, not about physical time.

---

## Examples (Domain-Agnostic)

### Example 1: Pure Computation
A program executes 10 deterministic steps.  
The step at **t = 7** is simply the eighth Delta in V.

### Example 2: Finance
A sequence of trades applied to an order book.  
Logical time corresponds to the order in which trades are applied,  
independent of exchange timestamps or network latency.

### Example 3: Biology
A chain of discrete reaction steps.  
Logical time tracks the position in the reaction sequence,  
not reaction rates or physical durations.

---

## Non-Examples

The following are not part of this time concept:

- wall clock timestamps (e.g. 2025-11-30 12:34:56)  
- system clocks or timers  
- block heights tied to real time  
- latency measurements  
- scheduling policies

These may be relevant in implementations,  
but they are outside the axiom.

---

## Relation to Behaviour and Governance

- Behaviour (V) provides the sequence.  
- Time (t) provides the index into that sequence.  
- Governance (G) and Boundaries (L) can refer to specific indices  
  or ranges in V, but they do not introduce their own time model.

Time is therefore:

- derived from V  
- monotonic in the index  
- sufficient to address any executed Delta

---

## Failure Modes Addressed
- FM-3 Logical time ambiguous or not index-based.
- FM-7 Observational time treated as semantic ordering (t_wall/t_perf used as t_index).

---

## Summary

Time in KL Execution Theory:

- is logical, not physical  
- is derived from behaviour  
- is discrete and index based  
- preserves ordering but not duration  
- carries no domain semantics by itself

It exists solely as a structural tool to refer to positions in the  
execution history.
