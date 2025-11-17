# Architecture – Deterministic Boundary Layer

## 1. Structural View

The DBL sits between three domains:

- **External Stimuli (non-deterministic)** → user input, models, agents  
- **Deterministic Control Plane** → policies, identity, logging  
- **Operational Systems (deterministic)** → infrastructure, data, workflows

```mermaid
flowchart LR
  A[External Request / AI Output] --> B(DBL Policy Gateway)
  B --> C[Execution Sandbox]
  C --> D[Infrastructure / System Actions]
  B --> E[Audit Chain]
