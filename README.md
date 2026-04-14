# Deterministic Boundary Layer for AI Governance

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

- [docs/MANIFEST.md](docs/MANIFEST.md)
- [docs/WHY_EXPLICIT_DECISIONS.md](docs/WHY_EXPLICIT_DECISIONS.md)
- [docs/SEE_IT_WORK.md](docs/SEE_IT_WORK.md)
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md)
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md)
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md)

---

## Active Boundary Topics

Current implementation work at the runtime boundary is mainly concentrated around:

- exposure modes
- integration path
- identity mapping
- tool-family gating
- request shaping
- economic shaping

Those topics belong to the live boundary surface, not to the architectural core.
For the concrete runtime shape, see [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway), especially:

- [CONTRACT_BOUNDARY.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/CONTRACT_BOUNDARY.md)
- [FIRST_INTEGRATION.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/FIRST_INTEGRATION.md)
- [INTEGRATION_SLICE.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/INTEGRATION_SLICE.md)
- [OIDC_INTEGRATION.md](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/OIDC_INTEGRATION.md)

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

---

## One sentence

> DBL lets you prove what was allowed, independent of what happened.
