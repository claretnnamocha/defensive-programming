# Boundaries and contracts

## Hostile boundary checklist

At every entry point (HTTP handler, message consumer, cron, UI submit):

- [ ] Parse and validate shape (schema, types)
- [ ] Reject unknown fields when strictness matters
- [ ] Scope identity (tenant, user, resource id) before use in queries
- [ ] Rate-limit or auth before expensive work
- [ ] Map errors to stable external codes — no stack traces to clients

**Barricade pattern (McConnell):** public API of a module checks all inputs; private methods may assume sanitized data.

## Trusted core checklist

Inside modules that only receive vetted data:

- [ ] Narrow contracts documented (what must hold)
- [ ] Assertions in dev/test for seam violations
- [ ] No duplicate validation on every private call — validate at seams only
- [ ] Internal errors indicate **programmer bug**, not user error

## Strip then stamp (identity boundaries)

Untrusted layers must not forward client-supplied identity or role headers. Remove spoofable headers; attach identity from verified session or service credential only.

## Wide vs narrow contract

| Wide contract | Narrow contract |
| --- | --- |
| Accepts all syntactic input; defines behavior for all | Preconditions required; violation = bug or hard error |
| Good for leaf utilities with clear math | Good for domain operations with state |

Avoid **widening** a narrow function to accept null and return 0 — that hides caller bugs (classic `strlen(NULL)` debate). Prefer: document precondition, assert in dev, fail at boundary.

## Service-to-service

- Stamp **service identity** on outbound calls
- **Fail loud** when required secret missing in production
- Do not assume gateway injects identity for background jobs