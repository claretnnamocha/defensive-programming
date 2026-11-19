# Review playbook

Use when **reviewing a diff or PR**.

## Order of attack

1. Read [diff-archetypes.md](diff-archetypes.md) — what kind of change is this?
2. Run [review-checklist.md](review-checklist.md) — integrity rules
3. For pure logic (no I/O): [logic-and-invariants.md](logic-and-invariants.md)
4. For failure paths: [assertions-vs-errors.md](assertions-vs-errors.md)
5. Flag [anti-patterns.md](anti-patterns.md)

## Finding format

Each issue:

- **What breaks and when** (concrete scenario)
- **Blocking?** yes / decision before merge
- **Smallest fix** (and whether a constraint removes the class of bug)

No generic "add more tests" without naming the missing case.