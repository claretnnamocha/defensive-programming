# Logic, invariants, and state

Use this playbook when changing **branches, loops, arithmetic, collections, state machines, reducers, or database transitions**.

## Preconditions and postconditions

| Kind | Who owns violation | Typical enforcement |
| --- | --- | --- |
| **Precondition** | Caller | Validate at boundary; assert at seam in dev |
| **Postcondition** | Callee | Assert before return; test proves |
| **Invariant** | Type/module | Hold across public methods; restored before return |

Document in comments or types when non-obvious:

```typescript
// Pre: amount > 0, account.status === 'active'
// Post: account.balance === oldBalance - amount
// Invariant: balance >= 0
```

If the caller breaks a precondition inside the trusted core, **fail loud** — do not "fix" caller bugs with silent clamps unless that is the explicit product rule at a boundary.

## Loop invariants

For every non-trivial loop, state the invariant:

- What is true **before** each iteration?
- What changes **monotonically** (index, sum, remaining work)?
- What proves **termination**?

Add an assertion or test that would fail if the loop corrupts the invariant (overflow, off-by-one, skipped element).

## State machines

- **One module** owns transitions; illegal transitions are errors, not no-ops.
- **Exhaustive handling** — discriminated unions and `never` in default branch for TypeScript.
- **Idempotent transitions** — duplicate event must not advance state twice (webhooks, retries).

## Branching discipline

- **Exhaustive `switch`** on enums/unions; no silent `default` that swallows new variants.
- **Else on else** — if a case should be impossible, assert or throw; do not fall through to a generic handler.
- **Feature flags** — defensive default when flag unknown (fail loud or documented safe default).

## Numeric and collection logic

- Guard **division by zero**, empty collection assumptions, `min`/`max` on empty.
- **Integer overflow** — assert or use safe types when values can grow without bound.
- **Sort/search preconditions** — sortedness, uniqueness; document if assumed.

## Partial updates

When updating one field of a coherent record, ask whether **sibling fields** must move together (status + foreign key, amount + currency). Prefer **transaction** or single update object.

## Tests that prove logic

| Assertion | Proves |
| --- | --- |
| Opposite branch | Fix-direction symmetry (bug did not move) |
| Second call same input | Idempotency |
| Invalid input at boundary | Rejection, not throw in core |
| Illegal transition | FSM rejects |

## Anti-patterns (logic)

- Magic default in `else` that masks missing case
- `?? 0` / `?? []` on failed fetch without typed error
- Reordering checks so common case bypasses validation
- Caching a failed resolution as permanent null for transient errors