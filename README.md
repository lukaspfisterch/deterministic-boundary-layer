# Deterministic Boundary Layers (DBL)

DBL is an architectural model for making **normativity explicit, replayable, and auditable**
under non-deterministic execution.

DBL separates **normative decisions** from **execution** by construction:
all normativity exists only as explicit **DECISION events** recorded in an
append-only event stream, while execution outputs are treated as observational
and non-normative.

This repository is the **conceptual landing page** for DBL.

---

## Conceptual foundation

DBL is **not** a foundational execution theory.

DBL builds on a strictly **non-normative execution substrate**.
That substrate is defined in **Execution Without Normativity**:

→ https://github.com/lukaspfisterch/execution-without-normativity

DBL assumes execution semantics and does not redefine them.
All guarantees of DBL apply **after** execution semantics are fixed.

---

## Core idea

DBL is built on a single central rule:

> All normative effects must be expressed explicitly as **DECISION events**
> in an append-only event stream **V**, independent of non-deterministic execution.

This rule applies once execution semantics are already defined by the
underlying non-normative substrate.

From this rule follow the core guarantees of DBL:

- deterministic governance under non-deterministic execution
- observational non-interference
- replayable and auditable normative state
- strict separation of concerns between admission, decision, and execution

DBL is an **architectural pattern**, not a policy framework and not a runtime.

---

## Model overview

DBL distinguishes four core components:

- **L – Boundaries**  
  Admit and shape authoritative inputs deterministically.  
  Boundaries constrain information flow but are not normative.

- **G – Governance**  
  Deterministically produces explicit **DECISION events** from authoritative inputs.  
  Governance has no access to observational data.

- **V – Event stream**  
  Append-only, immutable, totally ordered stream of events.  
  **DECISION events are the sole normative primitives.**

- **Execution / Effectors**  
  Execute actions after decisions are made.  
  Outputs, timing, errors, and traces are observational only.

Only **V** is authoritative for normative replay.

Read next:
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md)
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md)

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

Read next: [docs/GL_SEPARATION.md](docs/GL_SEPARATION.md)

---

## Repository map

DBL is defined by its formal model and axioms.
This repository acts as a **conceptual entry point and navigation hub**.

### Foundational theory

- **[execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity)**  
  Authoritative non-normative execution substrate.  
  DBL builds on this and assumes its semantics.

### Derived execution discipline (KL, non-normative)

- **[kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)**  
  Concrete, stateless execution kernel and discipline aligned with the foundational substrate.  
  Not DBL itself and not a competing theory.

- **[kl-execution-theory](https://github.com/lukaspfisterch/kl-execution-theory)**  
  Historical transitional axiomatic reference for KL concepts.  
  Preserved for context. Superseded as a canonical foundation by
  *Execution Without Normativity*.

### Architectural model (DBL)

- **[dbl-paper](https://github.com/lukaspfisterch/dbl-paper)**  
  Formal specification of DBL: definitions, axioms, claims, and proof sketches.  
  This is the **normative reference** for DBL.

- **[deterministic-boundary-layer](https://github.com/lukaspfisterch/deterministic-boundary-layer)**  
  Conceptual landing page and navigation hub (this repository).

### Reference semantics and validation

- **[dbl-reference](https://github.com/lukaspfisterch/dbl-reference)**  
  Reference-grade executable specification of DBL invariants.

  Acts as a semantic oracle and regression anchor:
  - validates DECISION primacy
  - enforces observational non-interference
  - defines normative replay and digest semantics
  - detects semantic drift in higher-level implementations

  This repository does not compose systems and does not provide infrastructure.
  It exists to make the DBL axioms executable, testable, and falsifiable.

### Core substrates

- **[dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)**  
  Reference implementation of **V**: append-only event stream,
  deterministic canonicalization, event and stream digests,
  normative projection.  
  Implements **V only**.

### DBL components and layering

- **[dbl-core](https://github.com/lukaspfisterch/dbl-core)**  
  Boundary-adjacent evaluation components and shared primitives
  used by higher layers.  
  Scope: support for L/G wiring, not a full system.

- **[dbl-policy](https://github.com/lukaspfisterch/dbl-policy)**  
  Versioned, explicit policy artifacts used by governance (G).  
  Scope: policy representation and release discipline, not execution.

- **[dbl-main](https://github.com/lukaspfisterch/dbl-main)**  
  Minimal reference composition wiring DBL components together
  (L, G, V integration).  
  Intended as a reference composition, not an application.

### Reference Domainrunners (proof by construction)

- **[dbl-voting-registry](https://github.com/lukaspfisterch/dbl-voting-registry)**  
  Minimal reference Domainrunner demonstrating DBL invariants
  in a concrete, intuitively normative domain.

  Demonstrates by construction:
  - strict separation of PROOF vs DECISION events
  - observational non-interference (digest invariance)
  - replayable normative state derived exclusively from V
  - deterministic behavior under non-deterministic checks

  Scope: demonstration and auditability, not feature completeness.

### Notes on authority and scope

- **dbl-paper defines DBL**. Code repositories do not redefine the model.
- Repositories are intentionally minimal and layered.
- “Listed here” does not mean “required”.

Read next:
- [docs/INTEGRATION.md](docs/INTEGRATION.md)
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md)

---

## Docs

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) – Layered model overview and roles
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md) – Boundary admission rules and information flow constraints
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md) – Governance lifecycle, versioning, and decision semantics
- [docs/GL_SEPARATION.md](docs/GL_SEPARATION.md) – Formal separation of G and L responsibilities
- [docs/INTEGRATION.md](docs/INTEGRATION.md) – Integration flow across DBL layers and repositories
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) – Threat assumptions and failure modes

Legacy notes live in `docs/legacy/` and are explicitly non-normative.

---

## What DBL is not

DBL explicitly does **not** provide:

- policy correctness or ethical guarantees
- execution determinism
- execution semantics or a foundational execution theory
- model alignment or training methods
- post-execution filtering as governance
- adaptive or learning-based policies
- distributed consensus or availability guarantees

If these assumptions are violated, DBL’s guarantees do not apply.

---

## Status

DBL is defined by its formal model and axioms.

Implementations are intentionally minimal and layered to preserve
determinism and auditability.

This repository is stable as a **conceptual and navigational entry point**.
