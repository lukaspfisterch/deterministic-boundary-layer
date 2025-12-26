# Integration

## Dependencies
- deterministic-boundary-layer depends on dbl-policy, dbl-core, and dbl-main.
- dbl-policy depends on dbl-core.
- dbl-main depends on dbl-core.
- No component depends on kl-kernel-logic directly in this layer.

## Direction of flow
- Authoritative inputs -> dbl-policy -> PolicyDecision.
- PolicyDecision -> dbl-core DECISION event appended to V.
- V -> dbl-main projection -> Phase, RunnerStatus.

## Event flow examples

Example 1: allow then execution
1) INTENT appended to V.
2) dbl-policy evaluates PolicyContext -> ALLOW.
3) DECISION(ALLOW) appended to V.
4) EXECUTION appended by kernel layer (outside this repo).
5) dbl-main projects V -> Phase.EXECUTED or Phase.PROVEN.

Example 2: deny
1) INTENT appended to V.
2) dbl-policy evaluates PolicyContext -> DENY.
3) DECISION(DENY) appended to V.
4) No EXECUTION events are appended for that intent.
5) dbl-main projects V -> Phase.DENIED.

Example 3: re-intent
1) INTENT appended to V.
2) DECISION(ALLOW) appended to V.
3) INTENT appended to V (new cycle).
4) dbl-main projects V -> Phase.INTENTED until a new DECISION appears.
