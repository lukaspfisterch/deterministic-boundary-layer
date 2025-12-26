# Deterministic Boundary Layer (DBL)

## Orientation

- DBL is a deterministic boundary and governance layer for systems that need explicit DECISION events.
- It solves auditability and replay by making V append-only and decisions fully traceable.
- In 2 minutes: run the demo UI and observe DECISION events and non-normative outputs.
- Start with the demo UI (dbl-boundary-service).
- Then read the axioms and contracts in the linked repos below.

DBL is a deterministic boundary and governance layer built on V as an append-only source of truth.

Normative effects are expressed only as DECISION events. Observations are non-normative and must not affect decisions. Replay and auditability follow from authoritative inputs and recorded decisions in V.

## Core model

| Symbol | Meaning |
| --- | --- |
| Δ | atomic action and observation |
| V | append-only event stream |
| t | logical order derived from V |
| G | governance, produces DECISION events |
| L | boundaries, constrain information flow and input shaping |

G and L are separated. L constrains inputs and information flow, G decides.

## Repository map

- **[kl-execution-theory](https://github.com/lukaspfisterch/kl-execution-theory)**
  Axiomatic foundation for Δ, V, and time.
- **[kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)**
  Deterministic execution core and trace contracts.
- **[dbl-core](https://github.com/lukaspfisterch/dbl-core)**
  Event substrate for V (INTENT, DECISION, EXECUTION, PROOF).
- **[dbl-main](https://github.com/lukaspfisterch/dbl-main)**
  Projection-only interpretation of V (Phase, RunnerStatus).
- **[dbl-policy](https://github.com/lukaspfisterch/dbl-policy)**
  Tenant scoped policy evaluation that emits DECISION events.
- **[dbl-boundary-service](https://github.com/lukaspfisterch/dbl-boundary-service)** (start here)
  Reference UI and service for boundary evaluation.
- **[kl-semantic-logic](https://github.com/lukaspfisterch/kl-semantic-logic)** (planned)
  Planned semantic anchoring and reconstruction model.

## How to explore

1) Run the demo UI (dbl-boundary-service).
2) Read the axioms (kl-execution-theory).
3) Read the contracts (kl-kernel-logic, dbl-core, dbl-policy, dbl-main).
4) Optional: planned semantic layer (kl-semantic-logic).

## Documents

- [ARCHITECTURE](docs/ARCHITECTURE.md)
- [BOUNDARIES](docs/BOUNDARIES.md)
- [GOVERNANCE](docs/GOVERNANCE.md)
- [INTEGRATION](docs/INTEGRATION.md)
- [THREAT_MODEL](docs/THREAT_MODEL.md)
- [GL_SEPARATION](docs/GL_SEPARATION.md)

## Scope

- This repo is a landing page and conceptual map.
- Detailed theory lives in kl-execution-theory.
- Implementation lives in the DBL and KL repos listed above.
