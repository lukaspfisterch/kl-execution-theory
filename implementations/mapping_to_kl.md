# Mapping to KL Kernel Logic

This document explains how KL Kernel Logic (KL) can be interpreted  
as a concrete implementation of KL Execution Theory.

The goal is to make the correspondence between the abstract axioms  
and the existing KL constructs explicit and stable.

---

## 1. Overview

KL Kernel Logic provides:

- `PsiDefinition` – declarative description of an operation  
- `PsiEnvelope` – transport and metadata container  
- `Kernel` – deterministic execution engine  
- `ExecutionContext` – execution parameters and policy hooks  
- `ExecutionTrace` – structured result of a run  
- optional policy and audit layers

KL Execution Theory provides:

- S – state space  
- Δ – atomic transitions  
- V – behaviour (sequence of Deltas)  
- t – logical time (index in V)  
- G – governance as f(V)  
- L – boundaries as g(V)

KL can be seen as one concrete realisation of this abstract model.

---

## 2. Mapping of Core Concepts

### 2.1 State (S)

In KL, the state space S is not a single fixed type.  
Instead, each operation can define its own notion of state  
through input and output structures.

Typical patterns:

- plain Python dictionaries (`Dict[str, Any]`)  
- dataclasses used as input / output types  
- domain-specific objects for foundations (e.g. order book, trajectories)

**Interpretation:**

- For a given operation, S is the set of all valid input states  
  that the Kernel accepts and transforms into outputs.

In composed systems (orchestrators), S may be the aggregate of:

- operation inputs  
- intermediate results  
- execution metadata  
- policy evaluation context

---

### 2.2 Delta (Δ)

In KL, a Delta corresponds to **one deterministic Kernel execution**  
of a specific `PsiDefinition` on a specific input.

Formally:

- select a PsiDefinition ψ  
- provide input state `x` (possibly combined with parameters)  
- execute `Kernel.run(ψ, x, context)`  
- obtain output state `y` and an ExecutionTrace

This entire run can be interpreted as:

**Δ : S → S**

where:

- S includes both input and output domains for that operation  
- Δ is defined by the pairing of ψ and its implementation

**Key requirement:**

- the operation implementation must be deterministic for KL to align  
  with the theory.

---

### 2.3 Behaviour (V)

Behaviour V in KL can be interpreted at multiple levels.

#### Level 1 – Single Operation Sequence

For a single PsiDefinition ψ executed multiple times:

- V is the sequence of Kernel runs with ψ and their corresponding inputs.

Example:

- repeated text simplification calls  
- repeated finance order book updates  
- repeated trajectory steps

Each run is one Δ.

#### Level 2 – Multi Operation Program

For a composed system:

- V is the ordered sequence of all Kernel runs across multiple ψ,  
  orchestrated by a controlling layer.

Example:

- a pipeline of foundation operations  
- a policy-enforced chain of computations  
- a multi step simulation using various Psi types

In both cases, V is the list:

**V = { Δ₀, Δ₁, …, Δₙ }**

where each Δᵢ is an executed Kernel operation.

---

### 2.4 Logical Time (t)

Logical time in KL corresponds to:

- the index of a Kernel execution within an ordered behaviour sequence.

If an orchestrator records all Kernel calls in a list,  
then the list index is t.

KL does not need any additional mechanism to represent time.  
It is sufficient to:

- record execution order  
- assign monotonic indices

---

### 2.5 Governance (G)

In KL, governance can be implemented as:

- a separate evaluation layer that inspects execution traces,  
  input / output bindings and envelope metadata.

Given a sequence of Kernel runs (behaviour) and their traces:

- G reads the collected traces  
- G applies deterministic rules / policies  
- G classifies behaviour (valid / invalid, allowed / disallowed, etc.)

Structurally, this is exactly:

**G = f(V)**

with V represented by the recorded chain of Kernel executions  
and their associated traces.

Governance must not:

- modify past traces  
- rewrite behaviour  
- alter execution after the fact

It remains purely evaluative.

---

### 2.6 Boundaries (L)

Boundaries in KL can be expressed as:

- derived metrics and constraint descriptions over execution history.

Examples:

- resource usage envelopes (time, memory, calls)  
- value ranges (e.g. numeric output bounds)  
- safety regions for model use  
- financial exposure envelopes in finance-related modules

In all cases, L is computed from V (behaviour) and its traces,  
and describes:

- which regions of behaviour are inside / outside acceptable ranges.

This corresponds directly to:

**L = g(V)**

where KL provides:

- trace data  
- structured outputs  
- stable IDs and timestamps

---

## 3. Role of Psi in the Mapping

`PsiDefinition` is the declarative description of a Delta family.

It does not execute anything on its own.  
It describes:

- operation type  
- domain  
- effect class (e.g. pure, stateful, external)  
- constraints and metadata

In KL Execution Theory terms:

- Psi defines the *shape* of Δ, not its execution.  
- Concrete executions of Psi by the Kernel are the actual Deltas.

Therefore:

- Psi is part of the specification of Δ  
- the Kernel plus implementation code realise Δ

---

## 4. ExecutionTrace as Concrete Trace (T)

KL typically returns an `ExecutionTrace` structure that may include:

- psi information  
- envelope metadata  
- success flag  
- input and output snapshots  
- timestamps  
- error messages (if any)  
- optional policy evaluation hooks

This can be mapped directly to the abstract trace concept T:

**T = { (t, s_t, Δ_t, s_{t+1}, meta_t) }**

where `meta_t` captures implementation-specific details,  
including environment, timing and auxiliary data.

The abstract theory does not constrain which metadata is stored,  
only that:

- the trace is coherent  
- it is sufficient to replay and evaluate behaviour  
- it aligns with deterministic execution.

---

## 5. Implementing Behaviour Capture in KL

To explicitly realise V and T in KL, an orchestrator can:

1. Maintain a list of execution records, e.g.:

   ```python
   behaviour = []
For each Kernel call, record:

t (index)

psi

input state

output state

ExecutionTrace

Append an entry:

python
Code kopieren
behaviour.append({
    "t": t,
    "psi": psi,
    "input": input_state,
    "output": result.output,
    "trace": result.trace,
})
After execution, pass behaviour to:

a governance evaluator G(behaviour)

a boundary evaluator L(behaviour)

This pattern gives KL an explicit, theory-aligned behaviour representation.

6. Determinism Requirements for KL
To fully align with KL Execution Theory, a KL-based system should:

Ensure that operation implementations are deterministic:

no uncontrolled randomness

no hidden global state

no time-dependent logic that affects results

Make environment dependencies explicit, for example:

encode configuration into the state or context

avoid referencing mutable global structures directly

Treat non-deterministic tasks as outside the core deterministic model,
or wrap them with explicit logging and normalisation.

This does not forbid non-deterministic components,
but it separates them from the deterministic core.

7. Governance and Boundaries as KL Modules
KL can host governance and boundary evaluation as separate modules:

kl_governance – implements G(V)

kl_boundaries – implements L(V)

These modules:

do not execute Kernel operations

only read behaviour and trace data

produce structured, deterministic evaluations

This preserves the separation between:

execution

evaluation

constraint description

in strict alignment with the theory.

8. Summary of Correspondence
The mapping between KL Execution Theory and KL Kernel Logic is:

S – concrete Python-level input / output state structures

Δ – individual deterministic Kernel runs of a Psi

V – ordered sequence of Kernel runs (behaviour)

t – index of each Kernel run in V

T – ExecutionTrace plus state snapshots

G – governance modules evaluating behaviour sequences

L – boundary modules describing constraint regions over behaviour

KL is therefore a concrete execution engine that can be seen as
one implementation of the abstract theory defined in this repository.

Future KL modules and orchestrators should aim to make this mapping
explicit, so that:

traces are stable

behaviour is replayable

governance and boundaries can be evaluated independently

the theory can serve as a reference for correctness and design.