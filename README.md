# Deterministic Boundary Layer for AI Governance

> DBL lets you prove what was allowed, independent of what happened.

Most AI systems can show prompts, traces, and logs, but still cannot prove what was explicitly allowed.
If an LLM or agent takes an action and the governing decision cannot be replayed independent of execution, the system is not auditable or defensible.
Logs are not proof. Decisions must be explicit.

DBL is for platform, infra, governance, and compliance engineers building LLM and agent systems where decisions must be reproducible, traceable, and verifiable before execution.

The model is simple:

```text
INTENT → DECISION → EXECUTION
```

Only `DECISION` is normative.
Execution remains non-deterministic and observational.
Governance happens before execution, and the decision can be replayed without re-running the model.

`deterministic-boundary-layer` is the public architectural entry point to that model.
It shows where authority lives across the DBL stack, how the repositories fit together, and how deterministic governance stays separate from execution.
In stack terms: DBL is the model, `dbl-gateway` is the runtime boundary, and `dbl-trading-zero` is the validation layer before authority.

---

## What this gives you

### Deterministic governance

Same admitted inputs, same decision.

### Replayable state

Reconstruct decisions without re-running execution.

### Auditability

Every outcome can be traced back to:

- admitted inputs
- policy version
- explicit decision

### No hidden authority

Execution, timing, telemetry, and side effects stay observational unless a boundary change explicitly admits them.

### Tamper-evident event chain

Events are cryptographically linked. The rolling chain digest can be recomputed
from the event sequence and verified end-to-end, independent of the runtime that
produced it.

---

## Repository map

### Theory

- [execution-without-normativity](https://github.com/lukaspfisterch/execution-without-normativity)
- [dbl-paper](https://github.com/lukaspfisterch/dbl-paper)

### Kernel and trace substrate

- [kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)
- [dbl-core](https://github.com/lukaspfisterch/dbl-core)
- [dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)

### Governance contract

- [dbl-policy](https://github.com/lukaspfisterch/dbl-policy)
- [dbl-policy-gates](https://github.com/lukaspfisterch/dbl-policy-gates)

### Runtime boundary

- [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway)

This repository sits above these components as the architectural map.
`dbl-gateway` now marks its core boundary-to-decision contract stable at `1.0.0`.

---

## Start here

Two documents, in order:

1. [docs/WHY_EXPLICIT_DECISIONS.md](docs/WHY_EXPLICIT_DECISIONS.md) — why implicit approval fails under LLM execution
2. [docs/SEE_IT_WORK.md](docs/SEE_IT_WORK.md) — a concrete walkthrough of an INTENT → DECISION → EXECUTION chain

To run a real request through the full chain in one minute, go to
[dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway) and run the Docker demo.

### Further reading

- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) — stack layout and component ownership
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md) — what crosses the boundary and what does not
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md) — the policy surface
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) — what DBL defends against and what it does not
- [docs/MANIFEST.md](docs/MANIFEST.md) — full document index

---

## Runtime boundary topics

This repository is the architectural map. The concrete runtime shape — exposure
modes, integration path, identity mapping via OIDC, tool-family gating, request
and economic shaping — lives in [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway):

- [CONTRACT_BOUNDARY.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/CONTRACT_BOUNDARY.md) — stable core contract vs evolving surface
- [FIRST_INTEGRATION.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/FIRST_INTEGRATION.md) — first request, decision read, replay
- [INTEGRATION_SLICE.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/INTEGRATION_SLICE.md) — raw send and inspection path
- [OIDC_INTEGRATION.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/OIDC_INTEGRATION.md) — OIDC token mapping with Entra example

---

## Selected research notes

This repository exposes a small public research surface.

These notes are:

- selected
- redacted
- observer-first
- non-authoritative

They show active validation work without exposing raw case data.

Start at [docs/RESEARCH_NOTES.md](docs/RESEARCH_NOTES.md).

