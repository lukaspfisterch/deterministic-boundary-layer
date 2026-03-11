# Deterministic Boundary Layers (DBL)

DBL is an experimental architecture and reference implementation for deterministic governance over non-deterministic systems.

It is not a product suite. It is a coded articulation of a model: a way to make governance explicit, replayable, and auditable without treating execution output as authority.

This repository is the conceptual landing page for that architecture.

> Only DECISION events are authoritative. Execution output never influences policy.

## Why DBL exists

Non-deterministic systems often collapse policy, execution, and observation into one runtime path. Once that happens, it becomes unclear what was authorized, why it was authorized, and whether the same decision could be replayed under the same rules.

DBL addresses that problem by separating governance from execution by construction. Policy becomes explicit. Decisions become append-only records. Execution remains observational.

## What this repository is

deterministic-boundary-layer is not the runtime system itself. It is the architecture hub for the model.

Its role is to explain:

- what DBL is
- how the layers relate to each other
- where authority lives
- why execution and governance must remain separate

If you want the formal model, start with dbl-paper. If you want the runtime path, follow the stack from kl-kernel-logic to dbl-gateway.

## Architecture

DBL can be understood as a layered stack:

```text
Theory
|
|- execution-without-normativity
`- dbl-paper

Execution substrate
|
`- kl-kernel-logic

Governance trace
|
|- dbl-core
`- dbl-vlog

Governance evaluation
|
|- dbl-policy
`- deterministic-boundary-layer

Runtime integration
|
`- dbl-gateway

Applications and experiments
|
|- axi
|- dbl-observer
|- dbl-chat-client
|- dbl-stack
`- domain runners
```

The architecture is intentionally narrow at the center. Most repositories are supporting experiments, interfaces, or demonstrations around a much smaller core.

## Core components

The minimal DBL architecture is expressed through four functional blocks.

- [kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)
  Deterministic execution substrate. It executes; it does not decide.

- [dbl-core](https://github.com/lukaspfisterch/dbl-core) and [dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)
  Governance trace substrate. They define and record the append-only event stream V with INTENT, DECISION, EXECUTION, and PROOF events.

- [dbl-policy](https://github.com/lukaspfisterch/dbl-policy)
  Explicit governance evaluation. It produces DECISION events from admitted inputs.

- [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway)
  Reference runtime integration. It connects intent ingestion, policy evaluation, execution, and trace writing.

## Theory and specification

The architecture depends on a prior execution model and a separate formal specification.

- [execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity)
  Foundational execution model. Defines the observational substrate on which DBL depends.

- [dbl-paper](https://github.com/lukaspfisterch/dbl-paper)
  Formal DBL specification. This is the authoritative statement of the model.

- [dbl-reference](https://github.com/lukaspfisterch/dbl-reference)
  Executable invariant validator and replay oracle for the architecture.

Code does not define DBL on its own. The model is specified separately and then pressure-tested through implementations.

## Authority model

Under DBL, not all events have the same status.

- DECISION events are authoritative.
- INTENT events establish context but are not authoritative.
- EXECUTION and PROOF events are observational.

Outputs, traces, timing, errors, and metrics must not influence governance unless they are explicitly admitted through a versioned boundary change.

This is the core separation behind the architecture: execution may be non-deterministic, but governance remains explicit and replayable.

## What DBL is not

DBL is not:

- a product suite
- a generic policy engine
- an execution theory
- a claim that execution itself is deterministic
- a substitute for formal correctness or ethics
- a post-hoc filtering scheme over runtime outputs

## Ecosystem

The wider repository set exists to explore, demonstrate, or operationalize the model. These repositories are useful, but they are not the architecture itself.

- [dbl-observer](https://github.com/lukaspfisterch/dbl-observer)
  Audit and timeline tooling for the event stream.

- [dbl-chat-client](https://github.com/lukaspfisterch/dbl-chat-client)
  Event-projection chat interface.

- [dbl-stack](https://github.com/lukaspfisterch/dbl-stack)
  Integrated stack for local end-to-end operation.

- [axi](https://github.com/lukaspfisterch/axi)
  Example system built around DBL ideas.

- Domain runners and other application repositories
  Experimental implementations of the architecture in concrete domains.

## Documentation

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — Layered model overview and component roles
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md) — Boundary admission rules and information flow
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md) — Governance lifecycle, versioning, and decision semantics
- [docs/GL_SEPARATION.md](docs/GL_SEPARATION.md) — Separation of G and L responsibilities
- [docs/INTEGRATION.md](docs/INTEGRATION.md) — Integration flow across layers and repositories
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) — Threat assumptions and failure modes
- [docs/CONSTITUTION.md](docs/CONSTITUTION.md) — Minimal constitutional statement of the architecture

Legacy notes in docs/legacy/ are historical and non-authoritative.
