# DBL Architecture v1.1

**Deterministic boundary for heterogeneous and nondeterministic systems**

Status: Working draft, detailed architecture specification

---

## 1. Purpose

This document defines a boundary architecture for integrating nondeterministic systems (AI models, external services, adaptive agents) into deterministic operational infrastructure.

**Goal:** Stabilize the operational perimeter around nondeterministic components without modifying their internal behavior.

---

## 2. Background

### Infrastructure Requirements

Classical deterministic infrastructure assumes:

- Reproducible execution for identical inputs
- Observable state transitions
- Causal event logs
- Bounded and predictable behavior
- Clear authority boundaries

### Nondeterministic System Characteristics

AI models, ML agents, and external black box services violate these assumptions:

- Internal states are not fully observable
- Identical inputs may produce different outputs
- Classical logging captures incomplete information
- Debugging and causal analysis break down
- Authority boundaries blur between model and infrastructure

### Architectural Gap

Operational systems require deterministic coordination. Nondeterministic subsystems require isolation, controlled interfaces, policy constraints, verified action paths, and full auditability.

A unified deterministic boundary layer is required.

---

## 3. Core Hypothesis

A deterministic boundary layer can safely coordinate nondeterministic components inside operational infrastructure by governing their external behavior without modifying their internal logic.

This layer wraps nondeterministic engines in a deterministic execution membrane.

---

## 4. Architecture Model

DBL consists of five cooperating components.

### Component Overview

```
External Request (nondeterministic)
    ↓
[1. Query Mediator]      normalize, validate, bind identity
    ↓
[2. Policy Sandbox]      evaluate policy, enforce boundaries
    ↓
[3. Execution Gate]      execute via KL Kernel or wrapped subsystem
    ↓
[4. Result Filter]       validate output, apply safety filters
    ↓
[5. Audit Layer]         record immutable trace
    ↓
Response (deterministic)
```

---

## 5. Component Specifications

### 5.1 Query Mediator

**Responsibility:** Normalize and prepare inbound requests for policy evaluation.

**Inputs:**
- Raw external request (HTTP, gRPC, message queue, etc.)
- Request metadata (headers, identity tokens, correlation IDs)

**Functions:**

1. **Input validation:**
   - Schema enforcement (JSON Schema, Protobuf, etc.)
   - Size and complexity limits
   - Injection detection (SQL, command, prompt injections)

2. **Request classification:**
   - Routing based on psi_type or intent
   - Domain and effect extraction
   - Criticality assignment

3. **Context binding:**
   - Identity extraction (user_id, tenant_id)
   - Request tracking (request_id, correlation_id, parent_trace_id)
   - Timestamp and session metadata

4. **Safety pre-filters:**
   - Block known malicious patterns
   - Enforce character set restrictions
   - Apply content policies (profanity, violence, etc.)

**Outputs:**
- Normalized request envelope (RequestEnvelope)
- Bound execution context (ExecutionContext)
- Routing decision (which execution path)

**Integration with KL:**

The Query Mediator constructs a PsiDefinition and ExecutionContext for KL Kernel Logic:

```python
from kl_kernel_logic import PsiDefinition, ExecutionContext, ExecutionPolicy

psi = PsiDefinition(
    psi_type=request.psi_type,
    domain=request.domain,
    effect=request.effect,
    schema_version="1.0",
    correlation_id=request.correlation_id,
    criticality=request.criticality,
)

policy = ExecutionPolicy(
    allow_network=False,  # determined by Policy Sandbox
    allow_filesystem=False,
    timeout_seconds=10.0,
)

ctx = ExecutionContext(
    user_id=request.user_id,
    request_id=request.request_id,
    policy=policy,
)
```

---

### 5.2 Policy Sandbox

**Responsibility:** Evaluate request against policies and enforce boundaries.

**Inputs:**
- RequestEnvelope (from Query Mediator)
- ExecutionContext (user, tenant, session)
- Policy database (RBAC, ABAC, custom rules)

**Functions:**

1. **Permission evaluation:**
   - Role-based access control (RBAC)
   - Attribute-based access control (ABAC)
   - Custom policy expressions (OPA, Cedar, etc.)

2. **Boundary enforcement:**
   - Network access (allow_network)
   - Filesystem access (allow_filesystem)
   - External service calls (allow_external_apis)
   - Execution timeout (timeout_seconds)

3. **Rate limiting and quotas:**
   - Per-user request limits
   - Per-tenant resource quotas
   - Global concurrency limits
   - Temporal constraints (time windows, cooldowns)

4. **Context consistency:**
   - Validate user is active and authorized
   - Check tenant subscription status
   - Verify session is not expired
   - Detect anomalous request patterns

**Outputs:**
- PolicyDecision (allowed: bool, reason: str)
- Enriched ExecutionPolicy (with resolved boundaries)
- Audit event (policy evaluation record)

**Decision Logic:**

The Policy Sandbox uses deterministic rule evaluation. All decisions are reproducible given the same inputs and policy version.

**Integration with KL:**

The Policy Sandbox populates ExecutionPolicy for KL:

```python
from kl_kernel_logic import ExecutionPolicy

# PolicySandbox determines these values
policy = ExecutionPolicy(
    allow_network=decision.allow_network,
    allow_filesystem=decision.allow_filesystem,
    timeout_seconds=decision.timeout_seconds,
)
```

If the decision is `allowed=False`, execution stops and an audit event is recorded.

---

### 5.3 Execution Gate

**Responsibility:** Execute request via deterministic substrate or wrapped nondeterministic subsystem.

**Execution Paths:**

**Path A: Deterministic Execution (via KL Kernel Logic)**

Used for:
- Pure functions (domain="math", effect="pure")
- Controlled I/O (domain="io", effect="read" or "io")
- Deterministic workflows

Integration:

```python
from kl_kernel_logic import CAEL, Kernel

# Use CAEL for policy-integrated execution
cael = CAEL()
trace = cael.execute(psi=psi, task=task, ctx=ctx)

# Or use Kernel for low-level control
kernel = Kernel()
trace = kernel.execute(psi=psi, task=task)
```

Output: ExecutionTrace with full timing, policy metadata, and outcome.

**Path B: Nondeterministic Execution (via wrapped subsystem)**

Used for:
- AI models (domain="ai", effect="external")
- External APIs (domain="external", effect="io")
- Black box services

Integration:

```python
# Wrap external subsystem
def wrapped_ai_call(prompt: str) -> str:
    response = external_ai_api.invoke(prompt)
    return response

# Execute via CAEL with correlation tracking
psi = PsiDefinition(
    psi_type="ai.chat.gpt4",
    domain="ai",
    effect="external",
    correlation_id=request.correlation_id,
)
trace = cael.execute(psi=psi, task=wrapped_ai_call, ctx=ctx, prompt=request.prompt)
```

Output: ExecutionTrace with timing, input/output hashes, and opaque correlation_id.

**Execution Properties:**

- All execution is isolated per request
- Timeout enforcement via ExecutionPolicy
- No direct operational authority for nondeterministic subsystems
- Full trace capture via KL

---

### 5.4 Result Filter

**Responsibility:** Validate and sanitize outputs before they reach operational systems.

**Inputs:**
- ExecutionTrace (from Execution Gate)
- Expected output schema
- Safety and compliance policies

**Functions:**

1. **Output schema validation:**
   - Type checking (JSON Schema, Pydantic, etc.)
   - Range and constraint validation
   - Required field verification

2. **Safety filters:**
   - Secret detection (API keys, passwords, tokens)
   - PII sanitization (email, SSN, phone numbers)
   - Injection prevention (SQL, command, XSS)

3. **Policy compliance:**
   - Content policies (profanity, violence, hate speech)
   - Data classification enforcement (public, internal, confidential)
   - Regulatory constraints (GDPR, HIPAA, SOC2)

4. **Consistency validation:**
   - Output matches expected effect (pure function did not mutate state)
   - Output is consistent with prior state or request
   - No cross-tenant data leakage

**Outputs:**
- Validated result or rejection (ValidationResult)
- Audit event (filter decision record)

**Example:**

```python
from dbl import ResultFilter, ValidationResult

filter = ResultFilter()
result = filter.validate(
    trace=trace,
    schema=output_schema,
    policies=compliance_policies,
)

if not result.valid:
    raise ValidationError(result.reason)
```

---

### 5.5 Audit Layer

**Responsibility:** Record all boundary events in an immutable log.

**Inputs:**
- RequestEnvelope (from Query Mediator)
- PolicyDecision (from Policy Sandbox)
- ExecutionTrace (from Execution Gate)
- ValidationResult (from Result Filter)

**Functions:**

1. **Event recording:**
   - Append-only event storage
   - Immutable audit trail
   - Cryptographic signing (optional)

2. **Correlation:**
   - Link events across request lifecycle
   - Support distributed tracing (trace_id, parent_trace_id, correlation_id)
   - Aggregate events by user, tenant, session

3. **Integration:**
   - Export to SIEM (Splunk, ELK, etc.)
   - Compliance reporting (SOC2, ISO 27001, etc.)
   - Forensic analysis tools

4. **Replay support:**
   - Capture sufficient metadata for request reconstruction
   - Enable policy replay (what would happen if policy changed?)
   - Support causal analysis and debugging

**Event Schema:**

```python
{
  "event_id": "uuid",
  "timestamp": "ISO 8601",
  "event_type": "request|policy|execution|validation|response",
  "trace_id": "uuid",
  "correlation_id": "uuid",
  "user_id": "str",
  "tenant_id": "str",
  "request_id": "str",
  "psi_type": "str",
  "domain": "str",
  "effect": "str",
  "policy_decision": {"allowed": bool, "reason": "str"},
  "execution_result": {"success": bool, "runtime_ms": float, "policy_result": "str"},
  "validation_result": {"valid": bool, "reason": "str"},
  "metadata": {}
}
```

**Integration with KL:**

The Audit Layer records ExecutionTrace directly from KL:

```python
from kl_kernel_logic import ExecutionTrace

audit.record_execution(trace=trace)

# Audit captures:
# - trace.trace_id, trace.parent_trace_id
# - trace.psi.describe()
# - trace.success, trace.runtime_ms, trace.policy_result
# - trace.policy_decisions
```

---

## 6. Integration Points

DBL integrates with:

**Identity and Access:**
- Active Directory (AD)
- Azure Active Directory (AAD)
- OAuth2 / OIDC providers
- SAML identity providers

**Logging and Observability:**
- SIEM (Splunk, ELK, Datadog)
- OpenTelemetry / Jaeger
- CloudWatch / Azure Monitor
- Custom audit backends

**Operational Systems:**
- ERP and line of business applications
- Databases (SQL, NoSQL)
- Message buses (Kafka, RabbitMQ)
- File storage and object stores

**Execution Substrates:**
- KL Kernel Logic (deterministic execution)
- AI model APIs (OpenAI, Anthropic, Azure OpenAI)
- External services (REST, gRPC, GraphQL)
- On-prem and edge inference hardware

**Policy Engines:**
- Open Policy Agent (OPA)
- AWS Cedar
- Custom rule engines
- Embedded policy evaluation

---

## 7. Benefits

- **Deterministic containment** of nondeterministic behavior
- **Clear separation** between model output and system authority
- **Full auditability** for compliance and forensics
- **Safe integration** into regulated infrastructure
- **Incremental adoption** without replacing existing systems
- **Platform-neutral** design for hybrid and on-prem deployments

---

## 8. Limitations

- **Increased complexity:** additional layer and operational overhead
- **Governance dependency:** requires mature policy and compliance practices
- **No model explainability:** DBL does not provide interpretability
- **No model replacement:** DBL does not compete with foundation models
- **Infrastructure dependency:** requires deterministic base layer and identity system

---

## 9. Next Steps

From v1.1 toward v2.0, refine:

- Policy evaluation models and conflict resolution
- Isolation mechanics (tenants, datasets, environments)
- Action signature schemas and provenance metadata
- Ledger structure and query patterns
- Risk scoring, escalation, and override procedures
- Integration templates for enterprise stacks
- Operational runbooks and incident response

---

## 10. Reference Implementation

See [KL Kernel Logic](https://github.com/lukaspfisterch/kl-kernel-logic) for the deterministic execution substrate that DBL wraps with governance and audit.

See [KL Execution Theory](https://github.com/lukaspfisterch/kl-execution-theory) for the minimal axioms (Δ, V, t, G, L) that DBL and KL implement.

---

## 11. Diagram

```text
+-------------------------------------------------------------------+
|                  Deterministic Boundary Layer (DBL)               |
+-------------------------------------------------------------------+

                     [1. Query Mediator]
+-------------------------------------------------------------------+
| - Input validation and normalization                              |
| - Request classification and routing                              |
| - Identity binding (user_id, tenant_id)                           |
| - Context construction (request_id, correlation_id)               |
+-------------------------------------------------------------------+
                              |
                              v
                      [2. Policy Sandbox]
+-------------------------------------------------------------------+
| - Permission evaluation (RBAC, ABAC)                              |
| - Boundary enforcement (network, filesystem, timeout)             |
| - Rate limiting and quotas                                        |
| - Context consistency checks                                      |
+-------------------------------------------------------------------+
                              |
                              v
                      [3. Execution Gate]
+-------------------------------------------------------------------+
| Path A: KL Kernel Logic (deterministic)                          |
| Path B: Wrapped subsystem (nondeterministic AI, external API)    |
| - Isolated execution per request                                  |
| - Timeout enforcement via ExecutionPolicy                         |
| - Full trace capture via KL                                       |
+-------------------------------------------------------------------+
                              |
                              v
                      [4. Result Filter]
+-------------------------------------------------------------------+
| - Output schema validation                                        |
| - Safety filters (secrets, PII, injections)                       |
| - Policy compliance (content, data classification)                |
| - Consistency validation                                          |
+-------------------------------------------------------------------+
                              |
                              v
                      [5. Audit Layer]
+-------------------------------------------------------------------+
| - Immutable append-only event log                                 |
| - Correlation across request lifecycle                            |
| - Integration with SIEM and compliance systems                    |
| - Forensic reconstruction and replay support                      |
+-------------------------------------------------------------------+

External Integration Points:
- Identity: AD, AAD, OAuth2, SAML
- Logging: SIEM, OpenTelemetry, CloudWatch
- Operational: ERP, databases, message buses
- Execution: KL Kernel Logic, AI APIs, external services
- Policy: OPA, Cedar, custom rule engines
```
