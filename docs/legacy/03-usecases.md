# Use Cases

DBL provides governance and boundary enforcement for nondeterministic systems. The following use cases demonstrate DBL wrapped around KL Kernel Logic or other execution substrates.

---

## Foundation: KL Kernel Logic

[KL Kernel Logic](https://github.com/lukaspfisterch/kl-kernel-logic) provides deterministic execution primitives. See [examples_foundations](https://github.com/lukaspfisterch/kl-kernel-logic/tree/main/src/kl_kernel_logic/examples_foundations) for atomic execution patterns.

DBL extends KL with policy mediation, validation, and audit for cross-system interactions.

---

## 1. AI Chat with Policy Enforcement

**Scenario:** User sends a prompt to an LLM. DBL enforces policy before and after model execution.

**Flow:**

1. **Query Mediator** normalizes prompt, binds user identity
2. **Policy Sandbox** checks: user has chat permission, prompt does not contain PII
3. **Execution Gate** invokes LLM via wrapped API
4. **Result Filter** validates: response is safe, no leaked credentials
5. **Audit Layer** records: user_id, prompt hash, model version, decision, response hash

**DBL Responsibilities:**
- Block prompts with sensitive data
- Rate limit per user
- Filter unsafe model outputs
- Log all interactions for compliance

---

## 2. Controlled Tool Execution in Production

**Scenario:** AI agent requests to execute a shell command on production infrastructure.

**Flow:**

1. **Query Mediator** parses tool call, extracts command and arguments
2. **Policy Sandbox** evaluates: is command whitelisted? Does user have privilege?
3. **Execution Gate** wraps KL Kernel execution:
   - PsiDefinition: psi_type="system.command", domain="infra", effect="io"
   - ExecutionPolicy: allow_filesystem=True, timeout_seconds=10
4. **Result Filter** validates: stdout does not contain secrets, exit code is expected
5. **Audit Layer** records: command, user, trace_id, runtime, policy_result

**DBL Responsibilities:**
- Prevent arbitrary command execution
- Enforce timeout and resource limits
- Capture full execution trace via KL
- Immutable audit for security review

---

## 3. Multi-Tenant AI with Isolation

**Scenario:** Multiple tenants share an AI service. Each tenant must be isolated.

**Flow:**

1. **Query Mediator** binds tenant_id from auth token
2. **Policy Sandbox** enforces: tenant-specific policies, resource quotas
3. **Execution Gate** invokes model with tenant context isolation
4. **Result Filter** ensures: no cross-tenant data leakage
5. **Audit Layer** records: tenant_id, request_id, correlation_id for tracing

**DBL Responsibilities:**
- Tenant isolation at boundary
- Per-tenant policy evaluation
- Cross-tenant audit trail
- Resource quota enforcement

---

## 4. Regulated AI Workflows (Finance, Healthcare)

**Scenario:** AI-assisted decision making in regulated environments.

**Flow:**

1. **Query Mediator** validates: request schema, required fields, data classification
2. **Policy Sandbox** checks: compliance policies, approval requirements
3. **Execution Gate** executes via KL Kernel or wrapped service
4. **Result Filter** validates: output schema, regulatory constraints
5. **Audit Layer** records: immutable evidence for auditors

**DBL Responsibilities:**
- Enforce compliance policies
- Validate inputs and outputs
- Generate audit-ready evidence
- Support forensic reconstruction

---

## Next Steps

Detailed specifications for each use case will include:
- Request and response schemas
- Policy definitions
- Integration with KL Kernel Logic
- Audit event structures
- Error handling and escalation

Expected update: Q1 2026.

---

## Reference Implementation

See [KL Kernel Logic](https://github.com/lukaspfisterch/kl-kernel-logic) for deterministic execution primitives that DBL wraps with governance and audit.
