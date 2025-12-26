# G/L-SEPARATION-CONTRACT (v1.0)

## Purpose
This contract defines the minimal, formal separation between Governance (G) and Boundaries (L)
within a deterministic execution model.  
It is **normative** for implementation, documentation, and architectural decisions.

---

## 1. Core principle: V is the sole source of truth

- All actions, decisions, and states are grounded exclusively in **V (Behavior)**.
- Neither Governance (G) nor Boundaries (L) may rely on implicit, hidden, or external assumptions.
- Any **normative** effect MUST be visible in V or explicitly justified by V.
- Deterministic **constraint enforcement** that does not introduce normative outcomes MAY operate
  without emitting events.

Clarification:
- *Normative* means: deciding what is allowed, forbidden, or required.
- *Constraint enforcement* means: restricting admissible information or flows without deciding outcomes.

---

## 2. Governance (G)

### Definition
Governance (G) is the **normative layer**.  
It determines *what* is allowed, forbidden, or required.

### Responsibilities of G
- G produces **DECISION** events in V.
- Each normative decision MUST be visible in V, including:
  - `policy_id`
  - `policy_version`
  - `tenant_id`
  - optional: justification or evidence
- G defines **content semantics**: which actions are permitted under which conditions.
- G consumes **only authoritative inputs** explicitly released by L.
- G is versioned, tenant-scoped, and follows a defined policy lifecycle.
- G follows a **default-deny** principle unless explicitly overridden.

### Not allowed
- G MUST NOT make observational data normative.
- G MUST NOT define or enforce information-flow constraints.
- G MUST NOT bypass or weaken Boundary rules defined by L.

---

## 3. Boundaries (L)

### Definition
Boundaries (L) are the **constraints layer**.  
They determine *which information* may enter decisions and *which outputs* may propagate.

### Responsibilities of L
- L controls information flow **before, during, and after** decisions.
- L defines and enforces:
  - allowed context schema
  - non-observability rules
  - input sanitization and shaping
  - output filtering
  - cross-tenant and cross-domain isolation
- L is **enforcement-capable**:
  - L MAY reject, truncate, mask, normalize, or shape inputs.
  - L MAY block or filter outputs.
- L MAY apply deterministic input or output transformations **without emitting events**,
  as long as no normative outcome is introduced outside V.
- L is **not normative in content**:
  - L does not decide *what should happen*.
  - L decides only *what information is admissible*.

### Not allowed
- L MUST NOT make normative decisions.
- L MUST NOT contain policy or business logic.
- L MUST NOT derive decisions from execution outcomes or observational data.
- L MUST NOT depend on derived or projected state (e.g. Phase, RunnerStatus).
- L MAY inspect the raw structure of V, but MUST NOT consume projections.

---

## 4. Separation of authoritative and observational data
- **Authoritative inputs**:
  - Explicitly defined fields allowed to influence DECISION events.
  - Released and shaped by L before reaching G.
- **Observational data**:
  - Telemetry, traces, debug output, logs, runtime measurements.
  - These MUST NOT become normative.
- L MUST ensure:
  - G cannot directly consume observational data.
  - Only authoritative inputs reach normative decision logic.

Principle:
> If data depends on non-deterministic execution behavior, it is observational and non-normative.

---

## 5. Boundary versioning (L)

- L configuration SHOULD be versioned.
- boundary_version SHOULD be recorded as authoritative metadata on admitted INTENT events.
- boundary_config_hash SHOULD be recordable alongside boundary_version as a deterministic hash over canonicalized boundary config.
- Any change to L that alters which inputs can reach G or how inputs are shaped (mask, normalize, truncate) SHOULD bump boundary_version and change boundary_config_hash.
- boundary_version is not a normative decision and MUST NOT imply allow or deny semantics.

Rationale: reproducibility depends on (policy_version, boundary_version, boundary_config_hash).

---



## 6. Projections and invalidity

- Projections derive solely from V and classify:
  - validity
  - invalidity
  - consistency or sequencing violations
- Projections are **descriptive**, not normative:
  - They do not enforce.
  - They do not modify V.
- Enforcement occurs only through **future actions** of G or L,
  derived from authoritative inputs and explicit rules.

---

## 7. Module structure (suggested)

Within a single repository (e.g. deterministic-boundary-layer),
two explicitly separated modules are maintained:

### Module G (Governance)
- Policy lifecycle management
- Normative DECISION logic
- Tenant scoping
- Policy versioning
- Default-deny semantics

### Module L (Boundaries)
- Context schemas
- Input and output constraints
- Non-observability guarantees
- Information-flow rules
- Deterministic enforcement mechanisms

A repository split is considered **only if**:
1. Boundaries (L) must be exported as an independent product, or
2. Governance and Boundary responsibilities are organizationally separated.

---

## End of Contract
