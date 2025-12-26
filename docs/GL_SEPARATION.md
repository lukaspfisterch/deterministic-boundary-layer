# G/L-SEPARATION-CONTRACT (DRAFT)

Purpose:
This contract defines the minimal, formal separation between Governance (G) and Boundaries (L)
within a deterministic execution model. It is normative for implementation, documentation, and
architecture decisions.

1. Core principle: V is the sole source of truth
- All actions, decisions, and states are grounded only in V (Behavior).
- Neither G nor L may rely on implicit or external assumptions.
- Any normative or restrictive effect MUST be visible in V or justified by V.

2. Governance (G)
Definition:
G is the normative layer. It determines what is allowed, forbidden, or required.
Responsibilities of G:
- G produces DECISION events in V.
- Each normative decision MUST be visible in V, including policy_id, policy_version, tenant_id, and optional justification or evidence.
- G defines content: which actions are permitted under which conditions.
- G consumes only authoritative inputs released by L.
- G is versioned, tenant-scoped, and follows a defined policy lifecycle.
Not allowed:
- G MUST NOT make observational data normative.
- G MUST NOT define information-flow constraints. That is the role of L.

3. Boundaries (L)
Definition:
L is the constraints layer. It determines which information may enter decisions and which outputs may propagate.
Responsibilities of L:
- L controls information flow before, during, and after decisions.
- L defines allowed context schema, non-observability rules, input sanitization and shaping, output filtering, and cross-tenant or cross-domain isolation.
- L is enforcement-capable: L may reject, truncate, mask, normalize, or shape inputs. L may block or filter outputs.
- L MAY apply deterministic input or output transformations without emitting events, as long as it does not introduce normative effects outside V.
- L is not normative in content. L does not decide what should happen, only what information is admissible.
Not allowed:
- L MUST NOT make normative decisions.
- L MUST NOT contain policy logic.

4. Separation of authoritative and observational data
- Authoritative inputs: explicitly defined fields allowed to influence DECISIONs and released by L.
- Observational data: telemetry, traces, debug data, logs. These MUST NOT become normative.
- L MUST ensure G cannot directly consume observables. Only authoritative inputs reach normative decisions.

5. Projections and invalidity
- Projections derive from V: validity, invalidity, and consistency classifications.
- Projections are descriptive, not normative. They do not enforce, and they do not change V.
- Enforcement occurs only through future G or L actions derived from authoritative inputs.

6. Module structure (suggested)
Module G: policy lifecycle, DECISION logic, tenant scopes, versioning, default deny.
Module L: context schema, input and output constraints, non-observability, information-flow rules, enforcement.
Repository split only if L needs to be exported or teams are separated.
