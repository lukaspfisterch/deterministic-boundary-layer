# Deterministic Boundary Layers (DBL)

What must exist for governance over non-deterministic systems to be explicit, replayable, and auditable?

DBL is an attempt to answer that question structurally. It separates governance from execution by construction, so that what was decided, why it was decided, and whether the same decision would be reached again are never ambiguous.

This repository is the conceptual landing page for that architecture.

> Only DECISION events are authoritative. Execution output never influences policy.

## DBL governance stack

```text
execution mechanics
    -> dbl-core

normative boundary
    -> dbl-policy

governance algebra
    -> dbl-policy-gates

domain governance
    -> tenant policies
```

Execution happens.
Decisions are recorded.
Policies are assembled.

`execution-without-normativity` shows that execution can exist without governance.
`dbl-policy-gates` shows how governance can be made composable, replayable, and structurally comparable.

## Structural premise

Policy, execution, and observation are often entangled in the same runtime path. When this happens, authority becomes implicit: it is no longer clear what was decided and what merely occurred during execution.

DBL defines a structure where this distinction is explicit. Policy is represented as append-only decision events. Execution remains observational.

## Architecture

The architecture answers three different questions at three layers:

| Layer | Question | Repositories |
|-------|----------|-------------|
| **Theory** | What must be true for deterministic governance to be possible? | [execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity), [dbl-paper](https://github.com/lukaspfisterch/dbl-paper) |
| **Core** | How is that structure technically guaranteed? | [kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic), [dbl-core](https://github.com/lukaspfisterch/dbl-core), [dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog) |
| **Governance** | How are explicit decisions represented and assembled? | [dbl-policy](https://github.com/lukaspfisterch/dbl-policy), [dbl-policy-gates](https://github.com/lukaspfisterch/dbl-policy-gates) |
| **Runtime** | How does this work in practice? | [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway), [dbl-reference](https://github.com/lukaspfisterch/dbl-reference) |

The full layer diagram is in [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

## Reference Implementation

The reference implementation is a stack, not a single repository.

```text
theory
execution-without-normativity

execution mechanics
dbl-core

normative boundary
dbl-policy

governance algebra
dbl-policy-gates

runtime reference
dbl-gateway
```

This is the shortest path from the theory to a running system.

## Reference Versions

The current reference stack is:

- `dbl-core 0.3.6`
- `dbl-policy 0.3.1`
- `dbl-policy-gates 0.1.1`
- `dbl-gateway 0.9.8`

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

## Documentation

- [ARCHITECTURE.md](docs/ARCHITECTURE.md) -- Layered model overview and component roles
- [BOUNDARIES.md](docs/BOUNDARIES.md) -- Boundary admission rules and information flow
- [GOVERNANCE.md](docs/GOVERNANCE.md) -- Governance lifecycle, versioning, and decision semantics
- [GL_SEPARATION.md](docs/GL_SEPARATION.md) -- Separation of G and L responsibilities
- [INTEGRATION.md](docs/INTEGRATION.md) -- Integration flow across layers and repositories
- [THREAT_MODEL.md](docs/THREAT_MODEL.md) -- Threat assumptions and failure modes
- [CONSTITUTION.md](docs/CONSTITUTION.md) -- Minimal constitutional statement
