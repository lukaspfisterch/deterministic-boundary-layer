# Start Here

This repository is small on purpose.

Use it in this order.

## 1. Fast orientation

- [../README.md](../README.md)
  What DBL is, what it is not, and why explicit DECISION events matter.

- [WHY_EXPLICIT_DECISIONS.md](WHY_EXPLICIT_DECISIONS.md)
  Why DBL treats explicit decisions as the core architectural move.

- [SEE_IT_WORK.md](SEE_IT_WORK.md)
  How to inspect one request and see the DBL chain in practice.

## 2. Core structure

- [ARCHITECTURE.md](ARCHITECTURE.md)
  Layer model, repository roles, and where this repo sits in the stack.

- [BOUNDARIES.md](BOUNDARIES.md)
  What may enter governance and what must remain observational.

- [GOVERNANCE.md](GOVERNANCE.md)
  Policy lifecycle, replay guarantees, and why DECISION is authoritative.

- [THREAT_MODEL.md](THREAT_MODEL.md)
  Typical ways systems collapse boundary discipline.

## 3. Runtime docking

- [dbl-gateway FIRST_INTEGRATION](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/FIRST_INTEGRATION.md)
  Smallest practical path from startup to one inspected DECISION.

- [dbl-gateway INTEGRATION_SLICE](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/INTEGRATION_SLICE.md)
  Raw send → decision → replay seam without client abstraction.

- [dbl-gateway OIDC_INTEGRATION](https://github.com/lukaspfisterch/dbl-gateway/blob/main/docs/OIDC_INTEGRATION.md)
  Token-to-decision identity mapping with a concrete Entra example.

## 4. Supporting separation docs

- [GL_SEPARATION.md](GL_SEPARATION.md)
  Clear split between governance (`G`) and boundaries (`L`).

- [INTEGRATION.md](INTEGRATION.md)
  How the repositories and layers connect in practice.

- [CONSTITUTION.md](CONSTITUTION.md)
  Minimal constitutional summary.

## 5. Public research surface

- [RESEARCH_NOTES.md](RESEARCH_NOTES.md)
  Redacted selected notes showing that empirical validation work is active.

## Legacy

Files in `docs/legacy/` are historical notes, not authoritative documentation.
