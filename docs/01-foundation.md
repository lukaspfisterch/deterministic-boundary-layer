# Foundation

## Problem Statement

Nondeterministic systems (AI models, external services, adaptive agents) violate core infrastructure assumptions:

- **No reproducibility:** identical inputs may yield different outputs
- **No traceability:** internal state is not fully observable
- **No causal continuity:** classical debugging and audit fail
- **No bounded behavior:** risk models cannot predict outcomes

Traditional infrastructure requires deterministic coordination. Nondeterministic components require isolation and governance.

---

## Scope

DBL defines a deterministic boundary around nondeterministic systems. It does not make the systems themselves deterministic. It makes their operational impact deterministic.

**In scope:**
- Policy-based request mediation
- Deterministic execution governance
- Result validation and filtering
- Immutable audit trails

**Out of scope:**
- Model explainability or interpretability
- Internal model behavior modification
- Deterministic model outputs
- Replacement of foundation models

---

## Architectural Position

DBL sits between three domains:

1. **Nondeterministic sources:** AI models, external APIs, human users
2. **Deterministic control plane:** policies, identity, audit
3. **Deterministic infrastructure:** databases, filesystems, networks, services

DBL ensures all transitions from domain 1 to domain 3 pass through domain 2.

---

## Relation to KL Execution Theory

[KL Execution Theory](https://github.com/lukaspfisterch/kl-execution-theory) defines minimal axioms for controlled execution:

- **Δ (atomic transition):** single state change
- **V (behavior):** sequence of transitions
- **t (logical time):** index in behavior
- **G (governance):** policy evaluation over behavior
- **L (boundaries):** constraint enforcement over behavior

**DBL implements G and L:**
- **G:** Policy Sandbox, Result Filter (governance)
- **L:** Query Mediator, Execution Gate (boundaries)

**KL Kernel Logic implements Δ:**
- Kernel.execute() provides atomic deterministic execution

DBL wraps KL and other substrates with governance and boundary enforcement.

---

## Design Principles

1. **External governance:** control behavior from outside, not inside
2. **Deterministic perimeter:** all boundary decisions are reproducible
3. **Audit by default:** every transition is logged immutably
4. **Zero direct action:** nondeterministic systems have no direct operational authority
5. **Policy as code:** all rules are explicit, versioned, testable

---

## Outcome

DBL produces a **deterministic operational footprint** for nondeterministic systems.

This footprint enables:
- Forensic reconstruction of decision paths
- Compliance with regulatory requirements
- Safe integration into regulated infrastructure
- Reproducible policy evaluation and audit

The nondeterministic system remains nondeterministic. The boundary around it is deterministic.
