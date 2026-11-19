# Assertions vs error handling

**McConnell rule:** Use **error handling** for conditions you **expect** might occur. Use **assertions** for conditions that **should never** occur if the program is correct.

| | Error handling | Assertions |
| --- | --- | --- |
| **Trigger** | Bad input, network down, conflict | Broken invariant, impossible enum, seam misuse |
| **Response** | Recover, return typed failure, retry if safe | Abort in dev; in prod: crash process or global handler — **fix code**, not data |
| **Audience** | User, operator, client | Programmer |
| **Production** | Always on | Often stripped (`NDEBUG`) or dev-only builds |

## When to assert

- Postcondition after mutation (balance non-negative)
- Loop invariant each iteration (in test builds)
- `never` branch reached
- Switch default on exhaustive union (compile-time + runtime in dev)
- Seam: parameter promised non-null by caller contract

## When to handle errors

- File not found, timeout, 409 conflict, validation failure
- External vendor 5xx (with retry policy if safe)
- User input fails schema

## Fail fast (Fowler)

When a problem occurs, **fail immediately and visibly**. Slow failure corrupts later steps and hides root cause.

Assertion messages should include **context** (ids, keys) — not repeat the boolean condition alone.

## Degraded mode (production)

If you cannot throw (batch job must continue):

- Name the mode in code and docs
- Log at WARN/ERROR with counter
- **Bound** how many records may degrade before abort
- Never degrade silently without observability