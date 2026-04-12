# Deterministic Boundary Layers

Make governance explicit.

`deterministic-boundary-layer` is the public entry point to the DBL architecture.
It shows where authority lives, how the pieces fit together, and why explicit DECISION events matter under non-deterministic execution.

---

## The problem

Most AI and agent systems cannot answer three simple questions reliably:

- Who asked what?
- What was actually allowed?
- Why did this outcome happen?

Policy, execution, and output are entangled.

That leads to three failures:

- authority becomes implicit
- replay becomes unreliable
- audit depends on logs, not structure

---

## The idea

Separate decision from execution.

Record what was allowed as an explicit event.

```text
INTENT → DECISION → EXECUTION
```

Only `DECISION` is authoritative.

Everything else is contextual or observational.

---

## The rule

> If something affects the outcome, it must appear in the DECISION.

If it does not appear there, it did not have authority.

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

## What DBL is not

DBL does not:

- make execution deterministic
- define policy correctness
- replace your models
- provide user or tenant management
- turn runtime observations into governance automatically

It defines where authority lives.

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
