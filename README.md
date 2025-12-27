# Deterministic Boundary Layers (DBL)

Deterministic Boundary Layers (DBL) is an architectural model for governing
non-deterministic systems with deterministic, auditable outcomes.

DBL separates **normative decisions** from **execution** by construction:
all normativity exists only as explicit **DECISION events** recorded in an
append-only event stream, while execution outputs are treated as observational
and non-normative.

This repository is the **conceptual landing page** for DBL.

---

## Core idea

DBL is built on a single central rule:

> All normative effects must be expressed explicitly as DECISION events in an
> append-only event stream **V**, independent of non-deterministic execution.

From this rule follow the core guarantees of DBL:

- deterministic governance under non-deterministic execution
- observational non-interference
- replayable and auditable normative state
- strict separation of concerns between admission, decision, and execution

DBL is an **architectural pattern**, not a policy framework and not a runtime.

---

## Model overview

DBL distinguishes four core components:

- **L — Boundaries**  
  Admit and shape authoritative inputs deterministically.
  Boundaries constrain information flow but are not normative.

- **G — Governance**  
  Deterministically produces explicit DECISION events from authoritative inputs.
  Governance has no access to observational data.

- **V — Event stream**  
  Append-only, immutable, totally ordered stream of events.
  DECISION events are the sole normative primitives.

- **Execution / Effectors**  
  Execute actions after decisions are made.
  Outputs, timing, errors, and traces are observational only.

Only **V** is authoritative for normative replay.

---

## Normativity and observation

Under DBL:

- **Normative**
  - DECISION events only

- **Contextual**
  - INTENT events (establish input context, non-normative)

- **Observational**
  - EXECUTION and PROOF events
  - outputs, traces, timing, errors, metrics

Observational data must not influence governance unless explicitly admitted
through a versioned boundary change.

---

## Repository map

DBL is defined by its formal model and axioms.
This repository serves as a conceptual entry point and navigation hub.

Authoritative and reference repositories:

- **dbl-paper**  
  Formal specification of DBL, including definitions, axioms, claims,
  and proof sketches. This is the normative reference for the model.

- **dbl-vlog**  
  Reference-grade implementation of the DBL event stream **V**:
  append-only log, deterministic canonicalization, event and stream digests,
  and normative projection. Implements **V only**.

Supporting and application-level repositories:

- **dbl-core**  
  Boundary-adjacent evaluation logic and supporting components.

- **dbl-boundary-service / execution gateways**  
  Reference applications demonstrating DBL integration.

---

## What DBL is not

DBL explicitly does **not** provide:

- policy correctness or ethical guarantees
- execution determinism
- model alignment or training methods
- post-execution filtering as governance
- adaptive or learning-based policies
- distributed consensus or availability guarantees

If these assumptions are violated, DBL’s guarantees do not apply.

---

## Status

DBL is defined by its formal model and axioms.
Implementations are intentionally minimal and layered to preserve determinism
and auditability.

This repository is stable as a conceptual and navigational entry point.

---

