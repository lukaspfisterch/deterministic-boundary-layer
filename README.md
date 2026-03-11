# Deterministic Boundary Layers (DBL)

Reading guide:
- To understand why DBL exists, read [**Why this exists**](#why-this-exists).
- For the architectural model, read [**Core model**](#core-model).
- To explore implementations, go to [**Repository map**](#repository-map).

DBL is an architectural model that makes **governance authority explicit, replayable, and auditable**
under non-deterministic execution.

DBL separates **authoritative decisions** from **execution** by construction:
all governance authority exists only as explicit **DECISION events** recorded in an
append-only event stream, while execution outputs are treated as observational only.

This repository is the **conceptual landing page** for DBL.

---

## Why this exists

Non-deterministic systems often mix policy decisions with runtime effects, making
authorization and accountability impossible to replay or audit. Once decisions are
hidden inside execution, divergent runs cannot be reconciled, and there is no single
source of truth for what was authorized or why.

DBL requires explicit, immutable decision records that precede execution.
Policy reasoning stays replayable, auditable, and isolated from observational noise.

---

## Core model

### Foundation

DBL is **not** an execution theory.

DBL builds on a strictly **observational execution substrate**.
That substrate is defined in **Execution Without Normativity**:

- https://github.com/lukaspfisterch/execution-without-normativity

DBL assumes execution semantics and does not redefine them.
All guarantees of DBL apply **after** execution semantics are fixed.

---

### Core rule

> All authoritative effects must be expressed as explicit **DECISION events**
> in an append-only event stream **V**, independent of non-deterministic execution.

This rule applies once execution semantics are already defined by the
underlying observational substrate.

From this rule follow the core guarantees:

- deterministic governance under non-deterministic execution
- observational non-interference
- replayable and auditable decision state
- strict separation of admission, decision, and execution

DBL is an **architectural pattern**, not a policy framework and not a runtime.

---

### Components

DBL defines four components:

- **L — Boundaries**
  Admit and shape authoritative inputs deterministically.
  Boundaries constrain information flow but do not make decisions.

- **G — Governance**
  Produces explicit **DECISION events** from authoritative inputs.
  Governance has no access to observational data.

- **V — Event stream**
  Append-only, immutable, totally ordered stream of events.
  **DECISION events are the sole authoritative primitives.**

- **Execution / Effectors**
  Execute actions after decisions are made.
  Outputs, timing, errors, and traces are observational only.

Only **V** is authoritative for decision replay.

Read next:
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md)
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md)

---

### Decisions and observations

Under DBL:

- **Authoritative**
  - DECISION events only

- **Contextual**
  - INTENT events (establish input context, not authoritative)

- **Observational**
  - EXECUTION and PROOF events
  - outputs, traces, timing, errors, metrics

Observational data must not influence governance unless explicitly admitted
through a versioned boundary change.

Read next: [docs/GL_SEPARATION.md](docs/GL_SEPARATION.md)

---

### What DBL is not

DBL does **not** provide:

- policy correctness or ethical guarantees
- execution determinism
- execution semantics or a foundational execution theory
- model alignment or training methods
- post-execution filtering as governance
- adaptive or learning-based policies
- distributed consensus or availability guarantees

---

## Repository map

### Foundational theory

- **[execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity)**
  Authoritative execution substrate.
  DBL builds on this and assumes its semantics.

### Execution kernel

- **[kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)**
  Stateless execution kernel derived from the foundational substrate.
  Provides deterministic execution traces consumed by higher layers.

### Specification

- **[dbl-paper](https://github.com/lukaspfisterch/dbl-paper)**
  Formal specification: definitions, axioms, claims, and proof sketches.
  This is the **authoritative reference** for DBL.

### Reference validation

- **[dbl-reference](https://github.com/lukaspfisterch/dbl-reference)**
  Executable specification of DBL invariants.
  Validates decision primacy, observational non-interference,
  decision replay, and digest semantics.

### Core substrates

- **[dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)**
  Reference implementation of **V**: append-only event stream,
  deterministic canonicalization, event and stream digests.

- **[dbl-core](https://github.com/lukaspfisterch/dbl-core)**
  Event substrate and shared governance primitives
  used by higher layers. Support for L/G wiring.

- **[dbl-policy](https://github.com/lukaspfisterch/dbl-policy)**
  Versioned, explicit policy artifacts used by governance (G).

### Integration

- **[dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway)**
  Reference integration implementation. HTTP gateway with
  multiple LLM providers (OpenAI, Anthropic, Ollama).
  Exposes `/capabilities`, `/ingress`, `/tail`, `/snapshot`.

- **[dbl-stack](https://github.com/lukaspfisterch/dbl-stack)**
  One-command full-stack setup (Gateway, Observer, Chat UI).

### Observation and UI

- **[dbl-observer](https://github.com/lukaspfisterch/dbl-observer)**
  Timeline and audit UI for the event stream.

- **[dbl-chat-client](https://github.com/lukaspfisterch/dbl-chat-client)**
  Event-projection chat UI.

### Reference domainrunner

- **[dbl-voting-registry](https://github.com/lukaspfisterch/dbl-voting-registry)**
  Demonstrates DBL invariants in a concrete domain:
  strict PROOF/DECISION separation, observational non-interference,
  replayable decision state, deterministic behavior under non-deterministic checks.

### Notes

- **dbl-paper defines DBL.** Code repositories do not redefine the model.
- Repositories are intentionally minimal and layered.
- "Listed here" does not mean "required."

---

## Docs

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — Layered model overview and component roles
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md) — Boundary admission rules and information flow
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md) — Governance lifecycle, versioning, and decision semantics
- [docs/GL_SEPARATION.md](docs/GL_SEPARATION.md) — Separation of G and L responsibilities
- [docs/INTEGRATION.md](docs/INTEGRATION.md) — Integration flow across layers and repositories
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) — Threat assumptions and failure modes

Legacy notes live in `docs/legacy/` and are explicitly non-authoritative.
