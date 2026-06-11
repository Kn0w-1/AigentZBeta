# Commit Brief: `77498cf` — RemixDialog: surface charging-persona + DVN balance + retry for diagnosis

| Field | Value |
|-------|-------|
| SHA | [`77498cf`](https://github.com/Kn0w-1/AigentZBeta/commit/77498cf9cfcba5942a6f2d2456a4aaaad37f0f04) |
| Author | Claude |
| Date | 2026-05-22T20:09:02Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
RemixDialog: surface charging-persona + DVN balance + retry for diagnosis

The user reports DVN-sufficient + wallet-connected scenarios where the
panel still asks them to pay on-chain. Likely root cause is a persona
mismatch — dialog debits persona A while the wallet UI is displaying
persona B's balance. Add visible diagnostics so the user can spot it:

  • New 'Charging persona: <label> · DVN: <amount>' strip at the top
    of PaymentIntentPanel. Persona label resolves from the canonical
    T1 surface (useActivePersona); the DVN figure comes from the
    server-side intent envelope.

  • 'Re-check' link in the strip re-runs submit() — useful after the
    user switches persona in the wallet drawer; we recompute DVN
    against the new persona without forcing a dialog close+reopen.

  • console.info('[RemixDialog] submit', { personaId, paymentMode,
    activePersonaLabel, surfacePersonaIdToken }) on every submit so
    operators can confirm the personaId in flight from DevTools.

No behaviour change — these are pure surface improvements to make the
persona-resolution path visible.
```

## Body

The user reports DVN-sufficient + wallet-connected scenarios where the
panel still asks them to pay on-chain. Likely root cause is a persona
mismatch — dialog debits persona A while the wallet UI is displaying
persona B's balance. Add visible diagnostics so the user can spot it:

  • New 'Charging persona: <label> · DVN: <amount>' strip at the top
    of PaymentIntentPanel. Persona label resolves from the canonical
    T1 surface (useActivePersona); the DVN figure comes from the
    server-side intent envelope.

  • 'Re-check' link in the strip re-runs submit() — useful after the
    user switches persona in the wallet drawer; we recompute DVN
    against the new persona without forcing a dialog close+reopen.

  • console.info('[RemixDialog] submit', { personaId, paymentMode,
    activePersonaLabel, surfacePersonaIdToken }) on every submit so
    operators can confirm the personaId in flight from DevTools.

No behaviour change — these are pure surface improvements to make the
persona-resolution path visible.

## Files Changed

| Change | File |
|--------|------|
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 1 file changed, 58 insertions(+), 1 deletion(-)
