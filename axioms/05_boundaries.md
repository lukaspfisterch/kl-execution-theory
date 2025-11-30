# Axiom 5: Boundaries (L)

Boundaries describe which regions of behaviour are permitted or forbidden.  
They do not control execution directly.  
They are derived descriptions of constraints over behaviour.

Boundaries provide a structural view on limits, capacities and  
acceptable ranges, independent of any specific domain.

---

## Definition

Given behaviour

**V = { Δ₀, Δ₁, Δ₂, …, Δₙ }**

boundaries **L** are defined as:

**L = g(V)**

where **g** maps the behaviour sequence to a representation of allowed  
and disallowed behavioural regions.

L may encode:

- limits  
- thresholds  
- invariants  
- safe regions  
- forbidden regions  
- saturation points

The exact format of L is domain-specific.  
The fact that it is derived from V is not.

---

## Properties

### 1. Behaviour-Derived
Like governance, boundaries are fully derived from behaviour.

L depends only on V.  
If a constraint is not reflected in V,  
it is not part of L.

---

### 2. Non-Executing
L does not:

- execute Deltas  
- modify V  
- enforce limits at runtime

It describes constraints, but does not apply them.  
Enforcement is a separate mechanism built on top.

---

### 3. Structural, Not Procedural
Boundaries are structural descriptions.

They do not specify:

- how to enforce limits  
- how to recover from violations  
- how to schedule execution

They only define:

- what is inside or outside acceptable behaviour  
- where saturation or break points exist  
- how behaviour is positioned relative to constraints

---

### 4. Domain Neutrality
L does not prescribe what specific constraints exist.

It only defines:

- that constraints can be expressed  
- that they are derived from behaviour  
- that they can be analysed or visualised

Domain logic decides what L represents  
(e.g. capital limits, concentration limits, safety margins).

---

## Interpretation

Boundaries represent:

- the shape of allowed behaviour  
- the edges of safe execution  
- the regions where constraints are active  
- the structure of "how far" behaviour can go

They make it possible to:

- reason about risk  
- reason about exposure  
- reason about stability  
- reason about saturation and breakdown

All based on V, not on external or hidden state.

---

## Examples (Domain-Agnostic)

### Example 1: Computational Resource Limits
L describes maximum memory usage, instruction counts or depth of recursion  
observed in V, and identifies regions that exceed predefined thresholds.

### Example 2: Financial Exposure
L captures position size, leverage, drawdown and other metrics derived  
from trade and position Deltas, highlighting regions where limits are  
respected or breached.

### Example 3: Biological Systems
L describes conserved quantities (e.g. mass balance) and critical ranges  
of concentrations, derived from sequences of reaction steps.

### Example 4: AI Systems
L encodes ranges of inputs, outputs or model usage patterns considered safe,  
based on observed execution traces.

---

## Non-Examples

The following are not boundaries in this sense:

- hard-coded limits inside execution code  
- imperative "if x > limit then abort" logic  
- external configuration files that are never reflected in V  
- informal descriptions of what "should be allowed"

These may define or influence constraints,  
but they are not L unless they are derivable from V.

---

## Relation to Governance (G)

Governance (G) and Boundaries (L) share the same input V,  
but serve different roles:

- G classifies behaviour (valid / invalid, compliant / non-compliant)  
- L characterises the structure of constraints and regions

G is judgement.  
L is geometry.

Both are:

- derived  
- non-executing  
- deterministic  
- domain-neutral

---

## Relation to Time and Behaviour

Since L is a function over V,  
it can:

- refer to all of V  
- refer to specific segments [t₁, t₂]  
- characterise evolving constraint usage over time

Time remains logical (index-based).  
Boundaries simply project constraints onto this time-indexed behaviour.

---

## Summary

Boundaries (L) in KL Execution Theory are:

- derived from behaviour  
- non-executing  
- structural, not procedural  
- domain-neutral  
- focused on allowed vs forbidden regions of behaviour  
- complementary to governance

Execution builds V.  
Governance evaluates V.  
Boundaries describe the constraint landscape induced by V.
