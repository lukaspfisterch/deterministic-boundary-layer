# See It Work

The fastest way to understand DBL is to inspect one request.

Not the output.

The decision.

---

## Step 1 — Send a request

Use `dbl-gateway`.

You are not looking for model quality first.

You are looking for structure.

---

## Step 2 — Inspect the chain

```text
INTENT → DECISION → PROOF → EXECUTION
```

Check what is recorded:

- who asked
- which trust class applied
- which tenant applied
- which tools were permitted
- which request constraints applied
- which economic constraints applied

---

## Step 3 — Focus on DECISION

This is the only authoritative event.

It should tell you:

- what was allowed or denied
- why
- which policy version applied
- which constraints were enforced

If you cannot answer these from the `DECISION`, the system is not explicit.

---

## Step 4 — Compare execution

Execution may:

- succeed
- fail
- drift
- surprise you

That does not change the decision.

This is the separation DBL enforces.

---

## Step 5 — Replay

Given the same inputs and policy version:

```text
same INTENT → same DECISION
```

No re-execution required.

---

## Where to go next

- `dbl-gateway` — runtime boundary
- `dbl-policy` — governance contract
- `dbl-core` / `dbl-vlog` — event substrate
- `dbl-paper` — formal model
