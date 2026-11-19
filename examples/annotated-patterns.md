# Annotated patterns (generic)

Illustrative only — adapt names and types to your stack.

## Strip then stamp (gateway)

```typescript
// Delete untrusted copies before attaching session-derived identity.
const headers = new Headers(incoming);
headers.delete('x-caller-user-id');
headers.delete('x-caller-role');
if (session) {
  headers.set('x-caller-user-id', session.userId);
}
```

## Fail-loud service config

```typescript
export function serviceAuthHeaders(): Record<string, string> {
  const key = process.env.SERVICE_API_KEY?.trim();
  if (!key) {
    if (process.env.NODE_ENV === 'production') {
      throw new Error('SERVICE_API_KEY required in production');
    }
    warnOnce('SERVICE_API_KEY missing; dev-only unauthenticated calls');
    return {};
  }
  return { 'X-Service-Secret': key };
}
```

## Discriminated ownership result

```typescript
type ResolveResult =
  | { ok: true; subjectId: string }
  | { ok: false; status: number; message: string };

async function resolveCaller(): Promise<ResolveResult> {
  if (!session) return { ok: false, status: 401, message: 'Unauthorized' };
  try {
    const upstream = await fetchSubject(session);
    if (upstream.status >= 500) {
      return { ok: false, status: 503, message: 'Upstream unavailable' };
    }
    // ...
  } catch {
    return { ok: false, status: 503, message: 'Upstream unavailable' };
  }
}
```

## Stale async guard (UI)

```typescript
useEffect(() => {
  let cancelled = false;
  (async () => {
    const data = await loadSlots(date);
    if (!cancelled) setSlots(data);
  })();
  return () => {
    cancelled = true;
  };
}, [date, refetchNonce]);
```

## Idempotent webhook sketch

```typescript
if (await store.isProcessed(event.id)) {
  return { received: true, duplicate: true };
}
await store.markProcessed(event.id);
await applyBusinessLogic(event);
```

## Exhaustive switch (logic)

```typescript
function next(state: OrderState, event: OrderEvent): OrderState {
  switch (event) {
    case 'paid':
      return state === 'draft' ? 'paid' : state;
  case 'ship':
      return state === 'paid' ? 'shipped' : state;
    default: {
      const _exhaustive: never = event;
      throw new Error(`unhandled event: ${_exhaustive}`);
    }
  }
}
```

## Loop invariant (test or assert)

```typescript
let fact = 1;
for (let i = 2; i <= n; i++) {
  // Invariant: fact === (i-1)!
  fact *= i;
}
```

## What to prove in tests

- Second webhook delivery does not double-apply
- 409/conflict on mutate does not trigger retry loop
- Forwarded headers do not include client `Authorization` cookie on outbound internal call
- Missing prod secret throws before first outbound call