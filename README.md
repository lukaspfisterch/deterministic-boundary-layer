# Deterministic Boundary Layer (DBL)

Deterministic boundary and governance layer built on V as an append-only source of truth. Normative effects are expressed only as DECISION events. Observations are non-normative and must not affect decisions. The model is replayable and auditable because decisions are derived from authoritative inputs and recorded in V.

## Core model

- Delta (Delta): atomic action and observation
- V (Behavior): append-only event stream
- t (Time): logical order derived from V
- G (Governance): produces DECISION events
- L (Boundaries): constrains information flow and input shaping
- G and L are separated; L constrains inputs, G decides

## Repository map

- kl-execution-theory: https://github.com/lukaspfisterch/kl-execution-theory
  Axiomatic foundation for Delta, V, and time.
- kl-kernel-logic: https://github.com/lukaspfisterch/kl-kernel-logic
  Deterministic execution core and trace contracts.
- dbl-core: https://github.com/lukaspfisterch/dbl-core
  Event substrate for V (INTENT, DECISION, EXECUTION, PROOF).
- dbl-main: https://github.com/lukaspfisterch/dbl-main
  Projection-only interpretation of V (Phase, RunnerStatus).
- dbl-policy: https://github.com/lukaspfisterch/dbl-policy
  Tenant scoped policy evaluation that emits DECISION events.
- dbl-boundary-service: https://github.com/lukaspfisterch/dbl-boundary-service
  Reference UI and service for boundary evaluation. You are here.
- kl-semantic-logic (planned): https://github.com/lukaspfisterch/kl-semantic-logic
  Planned semantic anchoring and reconstruction model.

## How to explore

1) Run the boundary-service demo to see DECISION events and observations.
2) Read kl-execution-theory for the axioms and invariants.
3) Study kl-kernel-logic, dbl-core, dbl-policy, and dbl-main for the implementation contracts.

## Documents

- [ARCHITECTURE](docs/ARCHITECTURE.md)
- [BOUNDARIES](docs/BOUNDARIES.md)
- [GOVERNANCE](docs/GOVERNANCE.md)
- [INTEGRATION](docs/INTEGRATION.md)
- [THREAT_MODEL](docs/THREAT_MODEL.md)
- [GL_SEPARATION](docs/GL_SEPARATION.md)
