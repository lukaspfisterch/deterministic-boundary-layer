# Architecture

This document explains the structural role of DBL.

The repository does not define a runtime product. It describes an architecture for making governance explicit under non-deterministic execution.

The core rule is simple:

> Only DECISION events are authoritative. Execution output never influences policy.

Everything in the stack is an unfolding of that rule.

## Layered model

DBL is best understood as a layered architecture.

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

The architecture is narrow on purpose. The conceptual center is small. Most repositories are integrations, demonstrations, or application-specific experiments built around that center.

## Theory

Theory defines the model before implementation.

- execution-without-normativity defines the observational execution substrate.
- dbl-paper defines the formal DBL model, including the separation between governance and execution.

This layer is prior to code. It explains what the architecture means.

## Execution substrate

The execution substrate is represented by kl-kernel-logic.

Its role is to execute atomically and deterministically within its own model of step order. It produces execution traces, but it does not decide what is allowed.

The kernel is execution physics, not governance.

## Governance trace

The governance trace is represented by dbl-core and dbl-vlog.

This layer defines and records the append-only stream V. The stream contains event kinds such as INTENT, DECISION, EXECUTION, and PROOF.

Its job is not to interpret policy but to preserve a deterministic, replayable, immutable record of what was admitted, decided, and observed.

Only the authoritative parts of V matter for governance replay.

## Governance evaluation

Governance evaluation is represented by dbl-policy together with the architectural rules described in this repository.

- dbl-policy evaluates admitted inputs and produces explicit DECISION outcomes.
- deterministic-boundary-layer defines the rules that constrain what inputs may be admitted, how governance remains isolated from observation, and how reproducibility is preserved.

This layer exists to keep authority explicit. It is where policy is evaluated, versioned, and constrained. It is not where execution is performed.

## Runtime integration

Runtime integration is represented by dbl-gateway.

This is the operational entry point that ties the layers together. It accepts intent, invokes governance, records decisions, executes actions, and writes observational events.

The runtime may orchestrate the path, but it must not collapse the distinction between decision and execution.

## Applications and experiments

Applications, observers, stacks, and domain runners are downstream artifacts.

They matter as demonstrations, tools, and experiments, but they are not the architecture itself. They sit on top of the core layers rather than defining them.

## Authority and information flow

DBL separates events by status.

- DECISION events are authoritative.
- INTENT events establish context.
- EXECUTION and PROOF events are observational.

This distinction is not cosmetic. It determines what may influence governance.

Outputs, traces, timing, errors, and telemetry must not affect policy unless they are explicitly admitted through a versioned boundary change. That is the information-flow boundary of the architecture.

## Role of this repository

deterministic-boundary-layer is the architecture hub for the governance layer.

Its role is to:

- define how boundaries constrain authoritative input
- define how governance remains separated from observation
- define how policy lifecycle and replay invariants are understood across the stack
- explain how the repositories fit together as one architecture

It is not:

- an execution engine
- a policy implementation library
- a projection engine
- a storage or transport layer
- a source of observational truth

## Authoritative companion documents

The architectural constraints described here are specified in more focused documents:

- [GL_SEPARATION.md](GL_SEPARATION.md) for the separation of governance and boundaries
- [BOUNDARIES.md](BOUNDARIES.md) for admission and information-flow constraints
- [GOVERNANCE.md](GOVERNANCE.md) for policy lifecycle and decision semantics
- [THREAT_MODEL.md](THREAT_MODEL.md) for failure assumptions and threat boundaries

These documents are the normative expansion of the architecture layer. Legacy notes do not override them.
