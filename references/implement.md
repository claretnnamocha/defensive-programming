# Implement — defensive logic workflow

Use when **adding or changing behavior**.

## Steps

1. **Surface** — boundary entry, core routine, async effect, or migration?
2. **Threats** — invalid input, double execution, stale async, wrong transition, void mismatch.
3. **Contract** — write pre/post/invariant in comment or type ([logic-and-invariants.md](logic-and-invariants.md)).
4. **Enforce** — validation at boundary; assertions at seams; transactions for multi-write.
5. **Errors** — typed failures; HTTP/status ladder if applicable ([layers.md](layers.md)).
6. **Concurrency** — cancellation, idempotency keys, version columns if needed.
7. **Prove** — test the opposite direction and the second call.

## Output checklist

- [ ] Failure paths loud
- [ ] No unreachable defensiveness without test
- [ ] Linked fields updated atomically
- [ ] Observability on new failure paths (log line minimum)