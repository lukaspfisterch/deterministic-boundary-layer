# Threat Model

## Risk categories
- Observational leakage: using traces or telemetry to drive decisions.
- Policy drift: inconsistent decisions for identical inputs.
- Hidden side effects: time, randomness, IO, or external state influencing outcomes.
- Ordering ambiguity: relying on timestamps instead of V order.

## Why determinism matters
- Determinism enables reproducible audit and compliance checks.
- Determinism prevents covert policy changes and silent regressions.
- Determinism ensures that V remains the sole source of truth.

## Typical failure patterns
- Using EXECUTION success or failure as a policy input.
- Using exception text or error codes to alter decisions.
- Inferring order from wall-clock timestamps instead of event index.
- Allowing policy changes without version bump.
- Mixing tenant contexts without explicit tenant_id.
- Treating projection outputs as success indicators.
