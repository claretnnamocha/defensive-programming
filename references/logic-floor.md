# Logic floor (read before editing behavior)

Minimum bar for **any** logic change — security, domain rules, or UI state.

## Before you write

1. Classify **boundary vs core** ([boundaries-and-contracts.md](boundaries-and-contracts.md)).
2. Name **pre/post/invariant** if state changes ([logic-and-invariants.md](logic-and-invariants.md)).
3. List **failure modes**: bad input, race, retry, partial update, overflow.
4. Decide **assertion vs error handling** ([assertions-vs-errors.md](assertions-vs-errors.md)).

## While you write

- No new silent swallow on catch paths.
- No retry on non-idempotent mutation without idempotency key.
- No lookup keyed only by "user" when product id / checkout id / entity id also defines identity.
- Exhaustive handling on unions and enums.

## Before you finish

- Negative test or assertion that fails if bug returns.
- If you added a guard, explain **reachability** or add test.
- If you added a default branch, confirm it cannot mask a new enum variant.

## Banned

- Catch-and-continue without log + contract
- DEBUG-only logging as sole "handling"
- Client-only authorization
- Unbounded skip-and-continue degradation