# Integration

## Dependencies
- execution-without-normativity defines the theory prior to code.
- dbl-core depends on kl-kernel-logic for execution substrate mechanics.
- dbl-policy depends on dbl-core.
- dbl-policy-gates depends on dbl-policy.
- dbl-gateway depends on dbl-core, dbl-policy, dbl-main, and dbl-ingress.
- dbl-main depends on dbl-core.

## Direction of flow
- Authoritative inputs -> dbl-policy -> PolicyDecision.
- Gate structure -> dbl-policy-gates -> root policy assembly.
- PolicyDecision -> dbl-core DECISION event appended to V.
- V -> dbl-main projection -> Phase, RunnerStatus.
- dbl-gateway ties the path together at runtime without owning policy semantics.

## Reference implementation path

The shortest implementation path from theory to runtime is:

1. `execution-without-normativity`
2. `dbl-core`
3. `dbl-policy`
4. `dbl-policy-gates`
5. `dbl-gateway`

## Reference versions

- `dbl-core 0.3.6`
- `dbl-policy 0.3.1`
- `dbl-policy-gates 0.1.1`
- `dbl-gateway 0.9.7`

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
