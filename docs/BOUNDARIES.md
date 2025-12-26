# Boundaries

## Boundary definitions
- Inbound boundary: inputs accepted for policy evaluation and decision recording.
- Outbound boundary: outputs emitted as DECISION events in V.

## Authoritative data
- PolicyContext inputs defined by dbl-policy.
- Event order in V.
- DECISION outcomes (ALLOW or DENY).

## Observational data (non-authoritative)
- Execution traces and trace fields (success, failure_code, exception_type, trace_digest).
- Runtime timing, wall-clock time, and other telemetry.
- Free-form error messages.

## Explicit bans
- Observational fields MUST NOT influence policy evaluation.
- Observational fields MUST NOT influence governance decisions.
- EXECUTION or PROOF events MUST NOT be used as inputs for policy outcomes.

## Boundary enforcement rules
- All decisions MUST be expressible as DECISION events in V.
- Policy outcomes MUST be deterministic for identical authoritative inputs.
- Any violation of boundary rules is a governance failure.
