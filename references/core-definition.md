# What defensive programming is

**Defensive programming** is the discipline of writing and reviewing logic so that **invalid assumptions become visible early**, **untrusted input cannot corrupt state**, and **replays and races cannot invent new truth**.

It applies to **any executable logic** — pure functions, domain rules, state machines, reducers, SQL, UI effects, batch jobs — not only security or payments.

## Synthesis (working definition)

> Protect the program from invalid data, impossible states, and collaborator mistakes. **At trust boundaries**, validate and fail closed. **Inside the trusted core**, document contracts (preconditions, postconditions, invariants) and enforce them in development. **On failure**, be loud: rethrow, typed error, or explicit degraded mode — never silent garbage out.

This skill merges:

| Tradition | Contribution |
| --- | --- |
| **McConnell — defensive programming** | Barricades, assertions vs error handling, protect from other modules' mistakes |
| **Meyer — design by contract** | Preconditions, postconditions, class invariants; caller vs callee responsibility |
| **Fowler — fail fast** | Visible immediate failure beats slow corruption |
| **Bloomberg — contracts / narrow API** | Do not widen input domain to "handle" abuse; detect misuse at boundaries |
| **Production engineering** | Idempotency, outbox, bounded retry, observability on every failure path |

## Scope of this skill

| In scope | Out of scope |
| --- | --- |
| Business rules, branching, loops, aggregation | Formatting, naming taste, framework churn |
| State transitions and FSMs | Pure visual design (unless logic in UI) |
| Concurrency, async, cancellation | Organizational process, staffing |
| Data integrity, migrations, upserts | |

## Two zones (always classify first)

1. **Hostile boundary** — HTTP, CLI, webhooks, files, queues, third-party APIs, browser input. **Validate everything.** Recover or reject with typed errors.
2. **Trusted core** — code that only runs after boundary sanitization. **Contracts + invariants.** Prefer narrow APIs; do not re-validate every internal call unless crossing a module seam.

See [boundaries-and-contracts.md](boundaries-and-contracts.md).

## Non-goals

- **Pessimistic coding everywhere** — redundant checks on every internal call obscure bugs (Bloomberg narrow-contract argument).
- **Unreachable defensiveness** — branches no execution path can hit; delete or prove with tests.
- **"Robust" silent recovery** — substituting default data without surfacing failure (McConnell's failure modes list) unless product explicitly chooses that path at a documented boundary.