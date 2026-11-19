# Anti-patterns

## Silent swallow

Empty catch, log-only at DEBUG, return `[]`/`null`/`0` on failure, or default that hides broken upstream. **Blocker** on new code.

## Unreachable defensiveness

Guard branches no caller can hit — adds complexity without proof. Either add a test showing reachability or remove.

## Retry on mutation

Automatic retry on booking, payment capture, or slot hold after 5xx can double-charge or double-book. Retry reads; surface error on writes unless idempotency key covers replay.

## Client-only security

Hiding a button is not authorization. Enforce on server.

## Wrong HTTP status

500 with empty body when dependency is down trains clients to misread outage as "no data". Prefer 503 with observable alert.

## Trust client headers

Accepting user-id or role headers without gateway verification and network isolation enables trivial spoofing.

## Hoist side effect without idempotency

Moving a write into a shared code path (both branches of `if`) without duplicate guard.

## Test the happy path only

Green test that does not assert new webhook field, header, or idempotency key invocation.

## Unbounded degraded mode

Skip-and-continue forever without threshold or operator-visible signal — slow-motion swallow.

## Cache upstream failure

Memoizing "lookup failed" as success or permanent null for the request lifetime when transient 503 should be retryable (distinguish negative cache of "no row" vs "error").