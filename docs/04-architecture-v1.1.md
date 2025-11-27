# Deterministic Boundary Layer (DBL)

Architecture Draft v1.1  
Deterministic boundary for heterogeneous and non-deterministic systems  
Status: Working draft, exploratory architecture note

---

## 1. Purpose

This document outlines an architectural approach for integrating heterogeneous and non-deterministic systems, including AI and ML models, into deterministic operational infrastructures.

The goal is to stabilise the boundaries around these components, not to change their internal behaviour.

---

## 2. Background

Traditional infrastructures rely on:

- deterministic execution  
- reproducible outcomes  
- traceable state transitions  
- causal logs  
- predictable operator behaviour  

Non-deterministic components violate these assumptions:

- internal states are not fully observable  
- identical inputs may produce different outputs  
- classical logging is incomplete  
- debugging paths break  
- authority boundaries blur  

The challenge is primarily architectural, not algorithmic.

---

## 3. Architectural Problem

Operational systems require deterministic coordination.  
Non-deterministic subsystems require:

- isolation  
- controlled input and output surfaces  
- policy-constrained behaviour  
- verified action paths  
- auditability  

Today, these controls are scattered or missing.  
A unified deterministic boundary between models and infrastructure is missing.

---

## 4. Core Hypothesis

A deterministic boundary layer is required to safely coordinate heterogeneous and non-deterministic components inside operational infrastructures.

This layer governs behaviour externally, without modifying model internals.  
It wraps non-deterministic engines in a deterministic execution membrane.

---

## 5. Proposed Architecture

The Deterministic Boundary Layer consists of six cooperating components.

### 5.1 Query Mediation Layer

Normalises and classifies inbound requests before they reach any non-deterministic engine.

**Functions**

- input validation  
- routing and fan-out selection  
- safety pre-filters  
- context binding  
- request isolation  

This layer connects directly to identity and policy sources and produces a normalised request envelope.

---

### 5.2 Policy Sandbox

Defines the permitted operational scope for each request.

**Functions**

- permission models  
- rate limits and quotas  
- environmental boundaries  
- deterministic rule evaluation  
- context consistency checks  

Only requests that pass the sandbox can reach the non-deterministic subsystem.  
All decisions are deterministic and reproducible.

---

### 5.3 Non-Deterministic Subsystem

The component whose internal reasoning cannot be deterministically reconstructed.

**Characteristics**

- probabilistic or adaptive behaviour  
- evolving models and parameters  
- incomplete internal traceability  
- no direct operational authority  

Examples include LLMs, ML models, rule engines with stochastic elements, or external black box services.

---

### 5.4 Result Verification Layer

Evaluates outputs from the non-deterministic subsystem before they touch any operational surface.

**Functions**

- schema and type checks  
- safety and policy filters  
- determinism guards, for example output range checks  
- consistency validation against prior state  
- signature and provenance requirements  

Only outputs that pass verification can progress to the action gateway.

---

### 5.5 Action Gateway

Provides the only authorised path through which operational actions are executed.

**Functions**

- enforce zero direct action from models  
- translate verified outputs into deterministic actions  
- sign actions through a deterministic authority component  
- maintain full audit logs of execution  
- expose quarantine and rollback paths  
- enforce operational boundaries and rate limits  

The gateway operates on infrastructure level identities and policies, not on model identities.

---

### 5.6 State and Audit Ledger

Records decisions, transitions and boundary events in an immutable form.

**Functions**

- append-only event history  
- support for reproducibility and replay  
- integration with governance and compliance systems  
- forensic reconstruction of decision paths  
- lifecycle transparency for requests, policies and actions  

This ledger is the primary evidence source for regulators, auditors and incident responders.

---

## 6. Integration Points

The Deterministic Boundary Layer interacts with:

- identity systems such as AD, AAD or WAM  
- logging, observability and SIEM pipelines  
- operational systems such as ERP and line of business applications  
- on-prem and edge inference hardware  
- security layers and policy engines  
- distributed data systems and message buses  

The design is platform-neutral and compatible with hybrid on-prem and cloud deployments.

---

## 7. Benefits

- deterministic containment of non-deterministic behaviour  
- improved auditability and accountability  
- alignment with regulated and safety-critical environments  
- clean separation between model output and system authority  
- compatibility with hybrid and on-prem infrastructures  
- incremental adoption inside existing environments  

---

## 8. Limitations

- increased architectural complexity and operational overhead  
- dependence on organisational governance and policy maturity  
- does not provide model explainability  
- does not replace or compete with large foundation models  
- depends on a deterministic base infrastructure and identity layer  

---

## 9. Next Steps

The next iterations, from v1.2 toward v2.0, will refine:

- policy evaluation models and conflict handling  
- isolation mechanics between tenants, datasets and environments  
- action signature schemas and provenance metadata  
- ledger structure and query patterns  
- risk scoring, escalation and override procedures  
- integration templates for typical enterprise stacks  
- operational safety procedures and runbooks  

---

## 10. Diagram

The following schematic illustrates the DBL components and their relationships.

```text
+---------------------------------------------------------------+
|                    Deterministic Boundary Layer               |
+---------------------------------------------------------------+

                   [ 5.1 Query Mediation Layer ]
+-------------------------------------------------------------------+
| - Input normalisation                                             |
| - Classification and routing                                      |
| - Safety pre-filters                                              |
| - Context binding                                                 |
+-------------------------------------------------------------------+
                              |
                              v
                       [ 5.2 Policy Sandbox ]
+-------------------------------------------------------------------+
| - Operational boundaries                                          |
| - Permissions and rate limits                                    |
| - Deterministic rule evaluation                                  |
| - Context consistency checks                                     |
+-------------------------------------------------------------------+
                              |
                              v
                  [ 5.3 Non-Deterministic Subsystem ]
+-------------------------------------------------------------------+
| - LLM, ML model, adaptive engine                                 |
| - Non-deterministic behaviour                                    |
| - No direct operational authority                                |
+-------------------------------------------------------------------+
                              |
                              v
                  [ 5.4 Result Verification Layer ]
+-------------------------------------------------------------------+
| - Schema and consistency checks                                  |
| - Safety filters                                                 |
| - Determinism guards                                             |
| - Signature requirements                                         |
+-------------------------------------------------------------------+
                              |
                              v
                        [ 5.5 Action Gateway ]
+-------------------------------------------------------------------+
| - Zero direct model action                                       |
| - Signed deterministic execution                                 |
| - Quarantine and rollback paths                                  |
| - Full audit of operational steps                                |
+-------------------------------------------------------------------+
                              |
                              v
                    [ 5.6 State and Audit Ledger ]
+-------------------------------------------------------------------+
| - Immutable event record                                          |
| - Traceability                                                    |
| - Forensic reconstruction                                        |
| - Lifecycle visibility                                           |
+-------------------------------------------------------------------+

External Integration Points:
 - Identity systems, for example AD, AAD, WAM
 - Logging and SIEM
 - Operational systems such as ERP and line of business
 - Security and policy engines
 - On-prem and edge inference nodes
