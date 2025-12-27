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

DBL is defined by its formal model and axioms. This repository is a conceptual entry point and navigation hub.

Normative specification

- **[deterministic-boundary-layer](https://github.com/lukaspfisterch/deterministic-boundary-layer)**  
  Landing page and conceptual hub (this repository).

- **[dbl-paper](https://github.com/lukaspfisterch/dbl-paper)**  
  Formal specification of DBL: definitions, axioms, claims, proof sketches.  
  This is the normative reference for DBL.

Core substrates

- **[dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)**  
  Reference implementation of V: append-only event stream, deterministic canonicalization,  
  event and stream digests, normative projection. Implements V only.

- **[kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)**  
  Kernel-level contracts and deterministic execution core concepts used by the ecosystem.  
  Not DBL itself, but the execution-theoretic foundation DBL is designed to govern.

- **[kl-execution-theory](https://github.com/lukaspfisterch/kl-execution-theory)**  
  Axiomatic foundation for Δ, V, and logical time. Provides the vocabulary and minimal contracts  
  that DBL builds on.

DBL components and layering

- **[dbl-core](https://github.com/lukaspfisterch/dbl-core)**  
  Boundary-adjacent evaluation components and shared primitives used by higher layers.  
  Scope: support for L/G wiring, not a full system.

- **[dbl-policy](https://github.com/lukaspfisterch/dbl-policy)**  
  Versioned, explicit policy artifacts used by governance (G).  
  Scope: policy representation and release discipline, not execution.

- **[dbl-main](https://github.com/lukaspfisterch/dbl-main)**  
  Composition layer wiring DBL components together (L, G, V integration) in a minimal,  
  reproducible manner. Intended as a reference composition, not an app.

Reference applications

- **[dbl-boundary-service](https://github.com/lukaspfisterch/dbl-boundary-service)**  
  Reference app demonstrating DBL-style invocation flow with explicit DECISION events and  
  observational separation. Useful for demos and practical integration examples.

- **[kl-exec-gateway](https://github.com/lukaspfisterch/kl-exec-gateway)**  
  Execution gateway and effector-facing integration. Demonstrates how to keep execution artifacts  
  observational while preserving replayability from V.

Notes on authority and scope

- dbl-paper defines DBL. Code repos do not redefine the model.
- Repos are intentionally minimal and layered. "Listed here" does not mean "required".

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
