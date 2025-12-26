# Architecture

## Layer stack
- dbl-core: deterministic event substrate with event kinds INTENT, DECISION, EXECUTION, PROOF.
- dbl-main: projection-only interpretation of V into Phase and RunnerStatus.
- dbl-policy: tenant-scoped policy evaluation producing DECISION events only.
- deterministic-boundary-layer (this repo): governance, boundary enforcement, and orchestration over policy and event flow.

## Role of deterministic-boundary-layer
- Define governance rules for policy lifecycle and enforcement boundaries.
- Orchestrate when and how policy decisions are produced and recorded.
- Enforce determinism and non-observability constraints across the stack.
- Treat V as the sole authoritative execution record.

## Normative contracts
The normative contracts for separation, governance, and boundaries are defined in
docs/GL_SEPARATION.md, docs/GOVERNANCE.md, and docs/BOUNDARIES.md. These files define
the authoritative constraints for this layer and supersede legacy notes.

## What this layer is NOT
- Not a task execution engine.
- Not a policy evaluation engine (delegated to dbl-policy).
- Not a projection engine (delegated to dbl-main).
- Not a storage, transport, or runtime system.
- Not a source of observational truth.
