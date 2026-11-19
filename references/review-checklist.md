# Review checklist — integrity

Use on every behavioral diff. Format findings as: **what breaks and when**, **blocking or not**, **smallest fix**.

## Idempotency

- Shared paths (create vs reuse, webhooks, retries) safe to run twice with same input
- Status/null filters on second call are reachable, not only on first invocation
- New writes hoisted to more branches include an explicit idempotency key or duplicate short-circuit

## Fix-direction symmetry

- Re-pointing IDs/refs removes the failure mode, not swaps it to the opposite common case
- State space enumerated for one-to-many → one-to-one narrowing
- Test covers the direction the fix did not obviously address

## Lookup scoping

- Dedup/void/link queries use every field that defines identity (product, campaign, checkout id, grant source — not only user/email)

## Partial updates

- Logically linked fields updated atomically or "null backfill only" — not half-old/half-new coherent states

## Refund / void / clawback

- Void query keys match what grant code stamped
- Callers pass fields required by the specific void branch

## Silent no-ops

- Warnings on "0 rows" only when an effect was expected

## Tests

- New side effects asserted with correct args
- Negative path would fail if bug returns — not only "does not throw"

## Trust boundaries

- Every new/changed internal HTTP call stamps service identity
- Client-supplied identity headers stripped before gateway re-stamps session
- Webhook routes verify signatures outside local dev when required

## Claim before vendor

- Durable claim/outbox before external payment, email, or fulfillment call
- Release claim on transport failure; compensate if vendor wins and claim loses

## Cross-service contracts

- Status enums, headers, and payload fields aligned across producers and consumers in the same release wave

## E2E honesty

- Integration tests use schema/fixtures that include columns the production query touches
- CI fails when dependencies unreachable — does not silently skip