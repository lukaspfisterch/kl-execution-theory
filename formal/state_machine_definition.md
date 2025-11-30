# State Machine Definition

This document defines the abstract state machine underlying  
KL Execution Theory. It connects the notions of state, Delta,  
behaviour and time into a single formal object.

The goal is to describe the minimal structure required for a  
deterministic, auditable and replayable execution system.

---

## 1. Components of the State Machine

A KL state machine **M** is defined as the tuple:

**M = (S, Σ, Δ, s₀)**

where:

- **S**: set of states  
- **Σ**: set of input symbols (optional, domain dependent)  
- **Δ**: set of atomic transitions  
- **s₀**: initial state

This is intentionally close to classical automata theory,  
but enriched with deterministic execution semantics.

---

## 2. States (S)

**S** is the set of all valid system states.

A state may encode:

- memory contents  
- configuration  
- domain-specific variables  
- intermediate results  
- internal control information

The theory does not constrain the structure of S.  
It only requires that Deltas can be applied to elements of S.

---

## 3. Input Symbols (Σ)

**Σ** is an optional set of input symbols.

It is used when the state machine interacts with external input  
in a structured way. For many internal execution models, Σ may  
be empty or implicit.

A symbol in Σ can represent:

- a user request  
- an external event  
- a message  
- a domain-specific stimulus

In the simplest form, Deltas may be defined without explicit Σ.

---

## 4. Deltas (Δ)

**Δ** is the set of atomic transitions.

Each Delta is a deterministic function:

**δ ∈ Δ**, with **δ : S → S**

In models that consume input symbols, a Delta can also be written as:

**δ : S × Σ → S**

but in KL Execution Theory, input handling is allowed to be encoded  
into the state itself, keeping the core definition:

**δ : S → S**

Deltas are:

- atomic  
- deterministic  
- domain-neutral at the abstract level

(See `axioms/01_delta.md`.)

---

## 5. Initial State (s₀)

**s₀ ∈ S** is the distinguished initial state of the machine.

All execution traces and behaviours are defined relative to s₀.

Different choices of s₀ may define different scenarios or initial  
conditions over the same underlying state space and Delta set.

---

## 6. Behaviour of the State Machine

Given a state machine **M = (S, Σ, Δ, s₀)** and a sequence of Deltas

**V = { Δ₀, Δ₁, …, Δₙ }**, with each Δᵢ ∈ Δ,

the behaviour of M under V is defined by the state sequence:

- **s₀** (initial state)  
- **s₁ = Δ₀(s₀)**  
- **s₂ = Δ₁(s₁)**  
- …  
- **sₙ₊₁ = Δₙ(sₙ)**

The pair

**(V, {s₀, s₁, …, sₙ₊₁})**

fully describes how M evolves under this behaviour.

This is a direct instantiation of the execution semantics described in  
`execution_semantics.md`.

---

## 7. Logical Time

Logical time **t** is defined as:

**t ∈ {0, 1, …, n}**

referring to the position in behaviour V.

At time t:

- the current state is **s_t**  
- the Delta that will be applied is **Δ_t** (for t ≤ n)  
- the next state is **s_{t+1}**

Time does not introduce new structure;  
it is a convenient index into the existing execution.

(See `axioms/03_time.md`.)

---

## 8. Extended State Machines

In practice, state machines may be extended with:

- labelled transitions  
- guards  
- internal control variables  
- error states  
- termination states

All of these can be encoded into S and Δ without changing the  
abstract definition of M.

For example:

- termination can be represented by a distinguished state s_term  
- errors can be represented by error states s_err ∈ S  
- control flow can be encoded as flags within S

The theory does not require special treatment of these cases.

---

## 9. Composition

Multiple state machines can be composed in various ways:

### 9.1 Parallel Composition

Two machines **M₁ = (S₁, Σ₁, Δ₁, s₀¹)** and  
**M₂ = (S₂, Σ₂, Δ₂, s₀²)**  

can be combined into a product machine:

**M₁×₂ = (S₁ × S₂, Σ₁ × Σ₂, Δ₁×₂, (s₀¹, s₀²))**

with Deltas acting on pairs of states.

Internally, this can still be represented as a single machine with  
composite state S' = S₁ × S₂ and appropriate Deltas Δ'.

### 9.2 Hierarchical Composition

A higher-level machine may treat entire traces or behaviours of  
lower-level machines as abstract Deltas.

This is useful for:

- layered execution  
- orchestration  
- policy-aware meta execution

The theory allows such constructions but does not depend on them.

---

## 10. Minimality

The definition of M is intentionally minimal:

- S for states  
- Σ for optional input  
- Δ for atomic transitions  
- s₀ for initial state

Combined with the axioms on behaviour, time, governance and boundaries,  
this is sufficient to model deterministic execution and its evaluation.

No additional structure is required at the theoretical level.

---

## 11. Summary

The state machine in KL Execution Theory is:

- a minimal deterministic automaton  
- defined by states, Deltas and an initial state  
- driven by sequences of Deltas (behaviour)  
- indexed by logical time  
- compatible with governance and boundary evaluation  
- extensible by encoding additional structure into states and Deltas

This provides the abstract backbone for concrete implementations such as  
KL Kernel Logic, DBL or any other deterministic execution engine that  
wishes to align with this theory.
