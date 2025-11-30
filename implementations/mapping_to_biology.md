# Mapping to Biology

This document describes how biological systems – in particular  
discrete biochemical processes and reaction chains – can be  
embedded into KL Execution Theory.

The goal is not to approximate real-world biophysics,  
but to show how biological structures can be represented as:

- states (S)  
- Deltas (Δ)  
- behaviour (V)  
- logical time (t)  
- governance (G)  
- boundaries (L)

---

## 1. Biological State (S)

A biological state **S** represents a snapshot of a system at a given  
level of abstraction.

Examples of state components:

- concentrations or amounts of molecules  
- occupancy of receptors  
- activation states of signalling pathways  
- gene expression levels  
- discrete cell states or population counts

In a minimal reaction step example:

- glucose amount  
- glucose-6-phosphate amount  
- ATP amount  
- ADP amount

Formally:

- S is the set of all valid configurations of these quantities.

The theory does not require physical units or kinetics.  
It only requires that the state is well-defined and that Deltas can  
transform it deterministically.

---

## 2. Biological Deltas (Δ)

In a discrete deterministic view, a Delta Δ represents a single,  
atomic biological event at the chosen level of abstraction.

Typical examples:

- a single reaction step in a metabolic pathway  
- activation or deactivation of a signalling component  
- a transcription event (turning a gene on or off)  
- a transition in a cell state machine (e.g. cell cycle phase change)

For a simple glycolysis step example:

- input: current amounts of glucose, glucose-6-phosphate, ATP, ADP  
- operation: apply one hexokinase step  
- output: updated amounts after one reaction unit

This can be modelled as:

**Δ_hexokinase : S → S**

where Δ_hexokinase encodes:

- glucose + atp → glucose_6_phosphate + adp

in a purely deterministic, discrete interpretation.

More complex systems may have multiple reaction Deltas:

- Δ₁: reaction A → B  
- Δ₂: reaction B → C  
- Δ₃: reaction with inhibition or activation logic  

Each remains an atomic transformation at the chosen abstraction level.

---

## 3. Behaviour (V) in Biology

Behaviour **V** in a biological context is:

- the ordered sequence of all applied biological Deltas.

Examples:

- the chain of reactions in a metabolic pathway  
- a sequence of signalling events in a cascade  
- steps in a cell state progression  
- discrete events in a simulation of a regulatory network

Formally:

**V = { Δ₀, Δ₁, …, Δₙ }**

with each Δᵢ representing one biological transition.

This sequence represents the *mechanical* evolution of the system,  
independent of physical time scales or reaction rates.

---

## 4. Logical Time (t) in Biology

Logical time **t** is the index into the biological behaviour sequence V.

At time t:

- the current state is **s_t**  
- the applied Delta is **Δ_t**  
- the next state is **s_{t+1}**

This is independent of:

- reaction rate constants  
- absolute physical time  
- continuous dynamics

Physical time and kinetics can be encoded as metadata or approximated  
by additional simulation layers, but logical time remains a purely  
structural notion.

This makes it possible to:

- reason about biological event ordering  
- compare alternative pathways  
- replay discrete biological scenarios  
- analyse sequences without full kinetic models

---

## 5. Governance (G) for Biological Systems

Governance **G** in biology evaluates behaviour V against constraints such as:

- conservation laws  
- stoichiometric balance  
- regulatory rules (e.g. allowed activation patterns)  
- safety or viability criteria in synthetic biology

Examples of governance checks:

- verifying mass balance across all reaction steps  
- ensuring that energy currency (e.g. ATP) is respected  
- checking that regulatory rules (activation, inhibition) are followed  
- validating that certain dangerous or non-viable states are never reached

Structurally:

**G = f(V)**

Where f:

- reads the sequence of biological Deltas  
- may reconstruct or use intermediate states induced by V  
- deterministically classifies behaviour as acceptable or unacceptable  
  under a given biological or synthetic design perspective

G does not change V or drive the system.  
It evaluates observed or simulated behaviour.

---

## 6. Boundaries (L) in Biology

Boundaries **L** describe the constraint landscape for biological behaviour.

Examples:

- minimum and maximum concentration ranges  
- viability ranges for key molecules  
- safe operating ranges for synthetic circuits  
- threshold regions where behaviour changes qualitatively

L can be derived from V by:

- computing the range of values visited for each state variable  
- identifying segments where constraints are approached or exceeded  
- describing regions of stable vs unstable behaviour  
- encoding envelopes of allowed biological variability

Formally:

**L = g(V)**

Where g provides a structural description of:

- which parts of behaviour stay within biologically acceptable regions  
- which parts move into failure, toxicity or non-viability regions

As in the theory, L:

- does not enforce constraints  
- does not execute Deltas  
- only describes the constraint geometry over behaviour

---

## 7. Example: Deterministic Glycolysis Step

Consider the minimal example:

- state: amounts of glucose, glucose-6-phosphate, ATP, ADP  
- Delta: `apply_hexokinase_step(state)`  
- output: updated BioState after applying one reaction unit

In KL terms:

- S: all possible `BioState` configurations  
- Δ: each invocation of `apply_hexokinase_step`  
- V: the sequence of such steps applied to the system  
- t: the index of each step in that sequence  
- G: evaluation of the sequence for conservation and correctness  
- L: derived envelopes for molecule amounts and safe ranges

This example can serve as a foundation module in a KL-aligned biology library.

---

## 8. Relevance for Biological Modelling

Embedding biology into KL Execution Theory provides:

1. **Structured discrete models**

   - clear event sequences  
   - explicit state transitions  
   - replayable behaviour

2. **Auditability of simulations**

   - traceable reaction histories  
   - reproducible scenarios  
   - independent verification of simulation engines

3. **Constraint analysis**

   - explicit envelopes for safe vs unsafe states  
   - structural views on viability regions  
   - reasoning about pathway behaviour without full continuous models

4. **Synthetic biology design**

   - design and evaluation of rule based biological systems  
   - governance over simulated biological circuits  
   - separation between execution and evaluation

---

## 9. Integration with KL Kernel Logic

In a KL based implementation:

- biological states are defined as dataclasses or structured dictionaries  
- biological Deltas are implemented as pure functions mapped to Psi  
- the Kernel executes these Deltas deterministically  
- an orchestrator records biological behaviour and traces  
- specialised modules compute G(V) and L(V) for biological scenarios

This allows KL to host:

- small educational biological examples  
- more complex discrete biological models  
- controlled simulations under governance and constraint evaluation

all aligned with the same execution theory.

---

## 10. Summary

Biological systems can be embedded into KL Execution Theory as:

- S: structured biological states (molecules, activations, cell states)  
- Δ: atomic reaction or state transition steps  
- V: ordered sequence of executed biological events  
- t: logical position in that sequence  
- G: evaluation of biological correctness, conservation and design rules  
- L: constraint landscape describing viable and non viable regions

This makes biology a second, structurally distinct domain that confirms  
the generality and robustness of the theory.
