# Mapping to KL Kernel Logic

This document explains how KL Kernel Logic (version 1.0) implements  
KL Execution Theory as a concrete, minimal execution substrate.

The goal is to make the correspondence between the abstract theory  
and the implementation explicit and stable.

---

## 1. Overview

KL Kernel Logic implements the 5-element execution chain:

```
Δ → V → t → G(V) → SS
```

**KL Kernel Logic provides:**

- `PsiDefinition` - declarative operation description  
- `PsiEnvelope` - versioned transport container  
- `Kernel` - atomic execution engine  
- `CAEL` - controlled execution with governance  
- `ExecutionContext` - execution parameters and policy  
- `ExecutionTrace` - complete execution record  
- `AuditReport` - governance and audit aggregation

**KL Execution Theory defines:**

- S - state space  
- Δ - atomic state transitions  
- V - ordered behaviour sequence  
- t - logical time (index in V)  
- G(V) - governance function over behaviour  
- SS - shadow state (persisted governance record)

KL Kernel Logic is a concrete realization of this abstract model.

---

## 2. Mapping the 5-Element Chain

### 2.1 State (S)

In KL, the state space S is not a single fixed type.  
Instead, each operation can define its own notion of state  
through input and output structures.

Typical patterns:

- plain Python dictionaries (`Dict[str, Any]`)  
- dataclasses used as input or output types  
- domain-specific objects for foundations (e.g. order book, trajectories)

**Interpretation:**

For a given operation, S is the set of all valid input states  
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
- execute `Kernel.execute(ψ, x, context)`  
- obtain output state `y` and an ExecutionTrace

This entire run can be interpreted as:

**Δ : S → S**

where:

- S includes both input and output domains for that operation  
- Δ is defined by the pairing of ψ and its implementation

**Key requirement:**

The operation implementation must be deterministic for KL to align  
with the theory.

---

### 2.3 Behaviour (V)

Behaviour V in KL can be interpreted at multiple levels.

#### Level 1: Single Operation Sequence

For a single PsiDefinition ψ executed multiple times:

V is the sequence of Kernel runs with ψ and their corresponding inputs.

Example:

- repeated text simplification calls  
- repeated finance order book updates  
- repeated trajectory steps

Each run is one Δ.

#### Level 2: Multi Operation Program

For a composed system:

V is the ordered sequence of all Kernel runs across multiple ψ,  
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

### 2.5 Governance Function (G(V))

In KL Kernel Logic, governance is implemented through:

- `PolicyEngine` interface - evaluates individual operations  
- `CAEL` layer - applies policy evaluation during execution  
- Governance evaluation over trace sequences

Given a sequence of Kernel runs (behaviour V) and their traces:

- G reads the collected ExecutionTrace objects  
- G applies deterministic policy rules  
- G classifies behaviour (allowed/denied, compliant/non-compliant, etc.)

Structurally, this is:

**G(V) : V → D**

where V is the behaviour sequence and D is the governance decision.

In KL, G(V) can be evaluated:

- **Before execution** - `PolicyEngine.evaluate(psi)` checks proposed operations  
- **During execution** - `CAEL` enforces policy at each Δ  
- **After execution** - retrospective governance over complete trace sequences

Governance remains purely evaluative and does not modify V.

---

### 2.6 Shadow State (SS)

Shadow State is the persistent governance record derived from G(V).

In KL Kernel Logic, SS is implemented through:

- `ExecutionTrace` objects - capture each Δ with governance metadata  
- `PolicyDecision` records - embedded governance decisions  
- `AuditReport` - aggregated governance data  
- Trace persistence - external storage (JSONL, database, audit logs)

Shadow State represents:

- Which policies were applied  
- Which operations were allowed or denied  
- Resource consumption patterns  
- Constraint satisfaction records  
- Complete audit trail

Formally:

**SS = G(V) evaluated and persisted**

KL provides SS through:

- Structured trace data with policy decisions  
- Serializable audit reports  
- Complete execution history for compliance verification  
- Deterministic replay capability

SS enables auditability without re-execution.

---

## 3. Role of Psi in the 5-Element Chain

`PsiDefinition` is the declarative specification of an operation.

It describes the intent of a transition without executing it:

- operation type (e.g. "math.add", "text.transform")  
- domain (e.g. "math", "text", "io")  
- effect class (e.g. "pure", "read", "io", "external", "ai")  
- constraints and metadata

In the 5-element chain:

- **Psi defines what Δ will do** (the operation specification)  
- **Kernel executes Δ** (atomic state transition)  
- **ExecutionTrace captures Δ in V** (behaviour sequence)

Therefore:

- Psi is the *specification* of Δ  
- Kernel + task function is the *realization* of Δ  
- ExecutionTrace is the *record* of Δ in V

---

## 4. ExecutionTrace in the 5-Element Chain

`ExecutionTrace` is the concrete record of a single Δ execution.

It captures:

- **psi** - the operation definition (Psi)  
- **envelope** - versioned transport container (PsiEnvelope)  
- **output** - result of the transition  
- **success** - whether execution completed  
- **error** - exception message if failed  
- **started_at**, **finished_at** - RFC 3339 timestamps (audit metadata)  
- **runtime_ms** - execution duration

Role in the chain:

- **ExecutionTrace records one Δ** (single atomic transition)  
- **Sequence of traces forms V** (behaviour)  
- **Trace index is t** (logical time)  
- **Trace contains governance metadata** (policy decisions)  
- **Trace contributes to SS** (shadow state persistence)

`ExecutionTrace` is therefore the fundamental building block  
of the complete 5-element chain in KL Kernel Logic.

The trace is:

- Complete (sufficient to replay and evaluate)  
- Deterministic (same inputs → same trace structure)  
- Serializable (can be persisted for audit)

---

## 5. Implementing Behaviour Capture in KL

To explicitly realize V and the 5-element chain in KL, an orchestrator can:

1. Maintain a list of execution records (V):

```python
behaviour = []  # V - behaviour sequence
```

2. For each Kernel call, record:
   - t (index)
   - psi
   - input state
   - output state
   - ExecutionTrace

3. Append an entry:

```python
behaviour.append({
    "t": t,                    # logical time
    "psi": psi,                # operation definition
    "input": input_state,      # pre-state
    "output": result.output,   # post-state
    "trace": result.trace,     # execution record
})
```

4. After execution, evaluate the complete chain:
   - **V** - the behaviour list itself
   - **G(V)** - governance evaluation over behaviour
   - **SS** - persist governance decisions and audit records

This pattern gives KL an explicit, theory-aligned implementation  
of the complete 5-element chain: Δ → V → t → G(V) → SS.

---

## 6. Determinism and the 5-Element Chain

For the 5-element chain to hold, operations must satisfy:

**For Δ (Atomic Transition):**
- Same input → same output (deterministic tasks)  
- No hidden global state  
- No uncontrolled randomness

**For V (Behaviour):**
- Total ordering preserved  
- All transitions recorded  
- No hidden execution paths

**For t (Logical Time):**
- Derived from sequence position  
- Monotonic and discrete

**For G(V) (Governance):**
- Deterministic evaluation (same V → same decision)  
- Non-executing (does not modify V)  
- Behaviour-derived only

**For SS (Shadow State):**
- Complete persistence of G(V) decisions  
- Queryable without re-execution

Non-deterministic operations (AI inference, external API calls)  
are supported but explicitly classified via effect classes.  
The chain structure holds; the determinism guarantee is scoped  
to the execution mechanism, not the task semantics.

---

## 7. Governance and Shadow State in KL

KL implements governance through:

- `PolicyEngine` - deterministic policy evaluation  
- `CAEL` - policy enforcement during execution  
- Governance modules - retrospective evaluation of behaviour sequences

Shadow State is captured through:

- `ExecutionTrace` - complete execution record per Δ  
- `AuditReport` - aggregated governance and compliance data  
- Trace persistence - external audit storage

These components:

- do not execute operations (remain evaluative)  
- only read behaviour and trace data  
- produce deterministic governance records

This preserves the separation between:

- execution (Kernel produces V)  
- governance (G evaluates V)  
- audit (SS persists G(V))

in strict alignment with the 5-element chain.

---

## 8. Summary of Correspondence

The 5-element chain mapping between KL Execution Theory and KL Kernel Logic:

| Theory | KL Kernel Logic Implementation |
|--------|-------------------------------|
| **S** | Python input/output state structures |
| **Δ** | `Kernel.execute()` call (atomic transition) |
| **V** | Ordered sequence of `ExecutionTrace` objects |
| **t** | List index + timing metadata (`runtime_ms`) |
| **G(V)** | `PolicyEngine` + governance evaluation over traces |
| **SS** | `ExecutionTrace` + `AuditReport` + trace persistence |

KL Kernel Logic implements the complete 5-element chain:

```
Δ → V → t → G(V) → SS
```

This mapping ensures:

- Traces are stable and complete  
- Behaviour is replayable (deterministic V)  
- Governance is evaluative (G(V) does not modify V)  
- Shadow State enables audit without re-execution  
- The theory serves as formal foundation for implementation correctness

KL Kernel Logic is a concrete, minimal execution substrate that  
directly realizes the abstract execution model defined in this repository.
