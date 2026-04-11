# Deterministic Boundary Layers

Make governance explicit.

`deterministic-boundary-layer` is the public jump page for the DBL architecture.
It explains where authority lives, how the repositories fit together, and why explicit DECISION events matter under non-deterministic execution.

---

## The problem

Most AI and agent systems cannot reliably answer three simple questions:

- Who asked what?
- What was actually allowed?
- Why did this outcome happen?

Policy, execution, and output are entangled.

That creates three failures:

- authority becomes implicit
- replay becomes impossible
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

Decisions can be reconstructed without re-running execution.

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

### Core substrate

- [kl-kernel-logic](https://github.com/lukaspfisterch/kl-kernel-logic)
- [dbl-core](https://github.com/lukaspfisterch/dbl-core)
- [dbl-vlog](https://github.com/lukaspfisterch/dbl-vlog)

### Governance contract

- [dbl-policy](https://github.com/lukaspfisterch/dbl-policy)
- [dbl-policy-gates](https://github.com/lukaspfisterch/dbl-policy-gates)

### Runtime boundary

- [dbl-gateway](https://github.com/lukaspfisterch/dbl-gateway)

This repository sits above those components as the architecture and navigation surface.

---

## Start here

- [docs/MANIFEST.md](docs/MANIFEST.md) for the shortest path through the material
- [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for the stack and layer roles
- [docs/BOUNDARIES.md](docs/BOUNDARIES.md) for admission and information flow
- [docs/GOVERNANCE.md](docs/GOVERNANCE.md) for policy lifecycle and replay rules
- [docs/THREAT_MODEL.md](docs/THREAT_MODEL.md) for what must not leak into governance

---

## Selected Research Notes

This repository also exposes a small public research surface.

These notes are:

- selected
- redacted
- observer-first
- non-authoritative

They are published to show that empirical validation work is active, without turning private research repos or raw case material into public dumps.

Start at [docs/RESEARCH_NOTES.md](docs/RESEARCH_NOTES.md).

---

## One sentence

> DBL makes it possible to prove what was allowed, independent of what happened.
