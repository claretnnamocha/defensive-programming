---
name: defensive-programming
description: >-
  Teaches and enforces defensive programming for any executable logic: pure functions,
  domain rules, state machines, loops, branches, async concurrency, APIs, webhooks,
  data migrations, and UI effects. Covers fail-fast, design-by-contract (pre/post/invariants),
  barricades at trust boundaries, assertions vs error handling, idempotency, and integrity review.
  Use when implementing, reviewing, debugging, or refactoring behavior where bad input, races,
  retries, or silent failure could corrupt state — not for formatting-only or docs-only edits.
license: MIT
compatibility: Cursor, Claude Code, Codex, Claude, OpenAI-compatible agents. Agent Skills format. Language-agnostic.
metadata:
  author: claretnnamocha
  version: "1.1.0"
argument-hint: "[implement | review | audit] [target or area]"
---

# Defensive programming

## Definition

**Defensive programming** makes invalid assumptions **visible early**, keeps **untrusted input** from corrupting state, and ensures **replays and races** cannot invent new truth. It applies to **all logic**, not only auth or payments.

Full synthesis: [references/core-definition.md](references/core-definition.md).

## Core principles

1. **Fail fast** — visible failure now beats corrupt state later (Fowler).
2. **Errors for expected faults; assertions for impossible states** (McConnell).
3. **Barricades at boundaries** — validate hostile input once; core trusts narrow contracts.
4. **Design by contract** — preconditions, postconditions, invariants at module seams.
5. **Loud failure paths** — rethrow, typed error, or named degraded mode; never silent defaults.
6. **Idempotency under replay** — webhooks, queues, double-submit.
7. **Prove the negative** — tests and reviews target the failure mode, not only happy path.

## Mandatory setup (each session with logic work)

1. Read [references/core-definition.md](references/core-definition.md) if definition is unclear.
2. **Classify** the work: boundary / core / concurrent / persistence — pick a playbook from the router below.
3. **Before editing behavior**, read [references/logic-floor.md](references/logic-floor.md).
4. On completion, scan [references/anti-patterns.md](references/anti-patterns.md).

## Playbook router

| Invoke as | Use when | Load first |
| --- | --- | --- |
| **implement** `[area]` | Writing or changing logic, handlers, effects, SQL, FSM | [references/implement.md](references/implement.md) + topic refs below |
| **review** `[target]` | PR, diff, code review | [references/review-playbook.md](references/review-playbook.md) |
| **audit** `[target]` | Deep integrity pass (money, auth, webhooks, migrations) | [references/review-checklist.md](references/review-checklist.md) + [references/diff-archetypes.md](references/diff-archetypes.md) |

### Topic references (load on demand)

| Topic | Reference |
| --- | --- |
| Definition and scope | [references/core-definition.md](references/core-definition.md) |
| Branches, loops, FSM, invariants | [references/logic-and-invariants.md](references/logic-and-invariants.md) |
| Boundary vs trusted core | [references/boundaries-and-contracts.md](references/boundaries-and-contracts.md) |
| Assert vs catch | [references/assertions-vs-errors.md](references/assertions-vs-errors.md) |
| Defense layers (prevent → prove) | [references/layers.md](references/layers.md) |
| Integrity review rules | [references/review-checklist.md](references/review-checklist.md) |
| Diff patterns | [references/diff-archetypes.md](references/diff-archetypes.md) |
| What not to do | [references/anti-patterns.md](references/anti-patterns.md) |
| Code sketches | [examples/annotated-patterns.md](examples/annotated-patterns.md) |
| Bibliography | [references/sources.md](references/sources.md) |

## Quick workflow (implement)

1. Name the **failure mode**.
2. **Boundary or core?** Validate at boundary; contract inside.
3. State **pre / post / invariant** if state changes.
4. **Assert vs error** for each failure path.
5. **Concurrency** — cancellation, idempotency, stale responses if async.
6. **Test** the opposite case and the second call.

## Error-path contract (always)

Every catch or failure path must:

1. Log WARN/ERROR and **rethrow**, or
2. Log and return a **typed failure** the caller must handle, or
3. Log and enter a **documented degraded mode** with observability.

Forbidden: catch-and-continue, silent `null`/`[]`/`0` defaults, hidden `??` fallbacks.

## When to skip

Copy, formatting, comments-only, or renames with **provably** no behavior change.