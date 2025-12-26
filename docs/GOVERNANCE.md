# Governance

## Policy lifecycle
- Each policy MUST have policy_id and policy_version.
- A policy update that changes decisions for identical inputs is a breaking change.
- Breaking changes MUST bump policy_version.

## Versioning rules
- Policy versions MUST be immutable once published.
- Rollbacks MUST be recorded as new versions.
- Effective policy_version MUST be explicit in all decisions.

## Audit and compliance
- All decisions MUST be recorded as DECISION events in V.
- Decisions MUST be traceable to policy_id, policy_version, and tenant_id.
- Audit trails MUST be reproducible from V without external state.

## Relationship to Boundaries
Governance defines the normative policy lifecycle and produces DECISION events, including versioning and default deny behavior. G MUST consume only authoritative inputs released by L and MUST NOT bypass boundary enforcement. See docs/GL_SEPARATION.md.

## Default failure modes
- Missing or invalid inputs default to DENY.
- Unknown policy version defaults to DENY.
- Observational data present in inputs is a governance violation and MUST result in DENY.
