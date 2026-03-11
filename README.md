# Deterministic Boundary Layers (DBL)

What must exist for governance over non-deterministic systems to be explicit, replayable, and auditable?

DBL is an attempt to answer that question structurally. It separates governance from execution by construction, so that what was decided, why it was decided, and whether the same decision would be reached again are never ambiguous.

This repository is the conceptual landing page for that architecture.

> Only DECISION events are authoritative. Execution output never influences policy.

## Structural premise

Policy, execution, and observation are often entangled in the same runtime path. When this happens, authority becomes implicit: it is no longer clear what was decided and what merely occurred during execution.

DBL defines a structure where this distinction is explicit. Policy is represented as append-only decision events. Execution remains observational.

## Architecture

The architecture answers three different questions at three layers:

| Layer | Question | Repositories |
|-------|----------|-------------|
| **Theory** | What must be true for deterministic governance to be possible? | [execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity), [dbl-paper](https://github.com/lukaspfisterch/dbl-paper) |
| **Core** | How is that structure technically guaranteed? | [kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic), [dbl-core](https://github.com/lukaspfisterch/dbl-core), [dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog), [dbl-policy](https://github.com/lukaspfisterch/dbl-policy) |
| **Runtime** | How does this work in practice? | [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway), [dbl-reference](https://github.com/lukaspfisterch/dbl-reference) |

The full layer diagram is in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Authority model

Not all events have the same status.

- **DECISION** events are authoritative.
- **INTENT** events establish context but are not authoritative.
- **EXECUTION** and **PROOF** events are observational.

Outputs, traces, timing, errors, and metrics do not influence governance unless explicitly admitted through a versioned boundary change.

Execution may be non-deterministic. Governance remains explicit and replayable.

## What DBL is not

- a product suite
- a generic policy engine
- an execution theory
- a claim that execution itself is deterministic
- a substitute for formal correctness or ethics
- a post-hoc filtering scheme over runtime outputs

## Ecosystem

These repositories explore, demonstrate, or operationalize the model. They are useful, but they are not the architecture itself.

- [dbl-observer](https://github.com/lukaspfisterch/dbl-observer) -- Audit and timeline tooling
- [dbl-chat-client](https://github.com/lukaspfisterch/dbl-chat-client) -- Event-projection chat interface
- [dbl-stack](https://github.com/lukaspfisterch/dbl-stack) -- Integrated local stack
- [axi](https://github.com/lukaspfisterch/axi) -- Example system built around DBL ideas

## Documentation

- [ARCHITECTURE.md](docs/ARCHITECTURE.md) -- Layered model overview and component roles
- [BOUNDARIES.md](docs/BOUNDARIES.md) -- Boundary admission rules and information flow
- [GOVERNANCE.md](docs/GOVERNANCE.md) -- Governance lifecycle, versioning, and decision semantics
- [GL_SEPARATION.md](docs/GL_SEPARATION.md) -- Separation of G and L responsibilities
- [INTEGRATION.md](docs/INTEGRATION.md) -- Integration flow across layers and repositories
- [THREAT_MODEL.md](docs/THREAT_MODEL.md) -- Threat assumptions and failure modes
- [CONSTITUTION.md](docs/CONSTITUTION.md) -- Minimal constitutional statement
