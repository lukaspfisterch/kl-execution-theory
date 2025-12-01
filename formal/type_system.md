# Type System

This document describes the abstract type system of  
KL Execution Theory.

The goal is not to define a programming language,  
but to make explicit which kinds of entities exist in the theory  
and how they relate to each other.

The types described here are conceptual categories,  
not concrete implementation types.

---

## 1. Core Types

The core types of the theory are:

- **State** (S)  
- **Delta** (Δ)  
- **Behaviour** (V)  
- **Time** (t)  
- **Trace** (T)  
- **Governance** (G)  
- **Boundaries** (L)

Each has a distinct role and is governed by the axioms.

---

## 2. State Type (S)

**Kind:** structural

**Meaning:**  
A State represents a complete description of the system at a given  
logical time.

**Notation:**  
- S is the set of all valid states.  
- s ∈ S is a particular state instance.

**Constraints:**

- S must be closed under Δ.  
- For every Δ : S → S and s ∈ S, the result Δ(s) ∈ S.

**Examples (conceptual):**

- S as a mapping from variable names to values  
- S as a structured record (order book, portfolio, memory image)  
- S as a composite of multiple subsystem states

The theory does not prescribe internal shape;  
it only requires that states are well-defined and transformable.

---

## 3. Delta Type (Δ)

**Kind:** functional

**Meaning:**  
A Delta is an atomic, deterministic state transformer.

**Notation:**

- Δ : S → S  
- Δ_set = { Δ₀, Δ₁, … } is the set of all allowed Deltas

**Constraints:**

- For all s₁, s₂ ∈ S, if s₁ = s₂, then Δ(s₁) = Δ(s₂)  
- Δ has no observable internal substructure

**Examples (conceptual):**

- a single VM instruction  
- a pure computation step  
- a single order book update  
- a single biochemical reaction step

---

## 4. Behaviour Type (V)

**Kind:** sequence

**Meaning:**  
Behaviour is a totally ordered sequence of executed Deltas.

**Notation:**

- V = { Δ₀, Δ₁, …, Δₙ }  
- V_set is the set of all finite behaviour sequences over Δ_set

**Constraints:**

- V is finite for any completed execution trace  
- V is totally ordered (no partial orders, no concurrency at this level)

**Examples (conceptual):**

- a program run as a sequence of operations  
- a trading day as a sequence of order book updates  
- a biological simulation run as a sequence of reaction steps

---

## 5. Time Type (t)

**Kind:** index

**Meaning:**  
Time is the index into behaviour.

**Notation:**

- t ∈ ℕ  
- 0 ≤ t ≤ n for V of length n + 1

**Constraints:**

- t is discrete  
- t is monotonic within a given behaviour sequence  
- time is defined only relative to V

**Examples (conceptual):**

- step index in an execution log  
- event index in a trading sequence  
- reaction index in a pathway simulation

---

## 6. Trace Type (T)

**Kind:** structured sequence

**Meaning:**  
A Trace captures both Deltas and the states they connect.

**Notation:**

- T = { τ₀, τ₁, …, τₙ }  
- Each τ_t = (t, s_t, Δ_t, s_{t+1}, meta_t)

where:

- t: logical time  
- s_t: pre state  
- Δ_t: applied Delta  
- s_{t+1}: post state  
- meta_t: optional metadata (implementation dependent)

**Constraints:**

- s₀ is the initial state  
- s_{t+1} = Δ_t(s_t) for all t  
- t matches the index in V

**Examples (conceptual):**

- a log of execution steps with full input/output  
- a detailed ledger of state transitions in a simulation

---

## 7. Governance Type (G)

**Kind:** evaluation result

**Meaning:**  
Governance is the result of evaluating behaviour (and optionally traces)  
 against rules or policies.

**Notation:**

- G = f(V)  
- or more concretely: G ∈ Γ, where Γ is a governance result type

Γ may include:

- boolean validity  
- structured classifications  
- lists of violations  
- scores or metrics

**Constraints:**

- f is deterministic  
- G depends only on V (and T if explicitly included)

**Examples (conceptual):**

- `G = { valid: true }`  
- `G = { valid: false, violations: [...] }`  
- `G = compliance_report`

---

## 8. Boundaries Type (L)

**Kind:** structural descriptor

**Meaning:**  
Boundaries describe constraints and regions over behaviour.

**Notation:**

- L = g(V)  
- or more concretely: L ∈ Λ, where Λ is a boundary description type

Λ may include:

- ranges and envelopes  
- constraint surfaces  
- thresholds and saturation points  
- regions labelled as safe/unsafe

**Constraints:**

- g is deterministic  
- L depends only on V (and derived states)

**Examples (conceptual):**

- `L = { max_position: 5.0, breaches: [...] }`  
- `L = { concentration_ranges: {...}, unsafe_regions: [...] }`

---

## 9. Meta Types

In addition to the core theoretical types, implementations introduce  
meta types that carry auxiliary information.

### 9.1 Metadata (meta_t)

**Kind:** auxiliary

Examples:

- timestamps  
- resource usage  
- environment identifiers  
- implementation specific diagnostics

The theory does not constrain these fields;  
they must not affect Δ, V, G or L in a non-deterministic way.

### 9.2 Policy Types

Policies may be used to parameterise governance or execution context.

Examples:

- resource limits  
- regulatory rule sets  
- safety specifications

Conceptually, policies are parameters to f(V) and g(V),  
not new core types of the theory.

---

## 10. Type Separation and Purity

The type system enforces conceptual separation:

- S and Δ define execution  
- V and T record execution  
- t indexes execution  
- G and L evaluate execution

No type is overloaded with multiple roles.

For example:

- Δ is never used as a governance result  
- G is never used as a state  
- L is never used as a Delta

This separation is essential to keep the theory minimal and clear.

---

## 11. Implementation Types

Concrete implementations (such as KL Kernel Logic) map these abstract types  
to programming language types:

- S → Python classes, dataclasses, dicts  
- Δ → functions or methods  
- V → lists of function identifiers or call records  
- T → structured execution traces  
- G → structured reports or result objects  
- L → data structures for constraint geometry

The mapping must preserve:

- determinism  
- ordering  
- separation of roles

---

## 12. Summary

The type system of KL Execution Theory introduces:

- States (S) as structural snapshots  
- Deltas (Δ) as atomic transformations  
- Behaviour (V) as ordered sequences  
- Time (t) as indices  
- Traces (T) as enriched sequences  
- Governance (G) as evaluation results  
- Boundaries (L) as constraint descriptions

These types, together with the axioms, form a minimal but expressive  
framework for reasoning about deterministic execution and its evaluation.
