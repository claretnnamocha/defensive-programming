# Diff archetypes

Review **files changed**, not commit messages. Strong defensive work often adds a **boundary module** plus tests in the same change.

| Archetype | What you see | Review focus |
| --- | --- | --- |
| **Greenfield boundary** | New guard/middleware/helper with allowlist + trust comment | Minimal public surface? Threat named? |
| **Strip then stamp** | Delete client identity headers; set from verified session | Spoofing path closed? |
| **Fail-loud config** | Prod throws if secret unset; dev warns once | Production cannot call auth-less? |
| **Ownership result type** | `ok: false` + typed HTTP response ladder | 503 vs 403 vs 404 distinguished? |
| **Stale async guard** | `cancelled` flag + effect cleanup; refetch nonce | Session errors vs stale data handled correctly? |
| **Idempotent vs retry** | Tests assert single attempt on mutating 5xx | Retry only on safe reads? |
| **Spec wall** | Large new test file beside small prod delta | Discriminating assertions, not smoke only? |
| **CI gate** | Lint/e2e wiring with fail-not-skip | Regression enforced in CI? |
| **Anchored allowlist** | Regex public routes with `$`; per-route justification | PII-free or token-gated? |
| **Negative cache** | Per-request memo: undefined / value / failed | Failed lookup not retried N times per request? |

When a diff does not match an archetype but touches money, auth, or webhooks, still run the [review-checklist.md](review-checklist.md).