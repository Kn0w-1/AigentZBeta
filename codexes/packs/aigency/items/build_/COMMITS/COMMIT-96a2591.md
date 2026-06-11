# Commit Brief: `96a2591` — fix Save to myCanvas blocked when client has no personaId

| Field | Value |
|-------|-------|
| SHA | [`96a2591`](https://github.com/Kn0w-1/AigentZBeta/commit/96a259197df4be69b98926cfbddd7c8d370ac398) |
| Author | Claude |
| Date | 2026-05-22T05:32:29Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix Save to myCanvas blocked when client has no personaId

With sign-in gating disabled, RemixDialog often runs without a
client-side personaId (identity flows server-side via the Bearer
token + spine). The Save button kept the !personaId guard from the
initial scaffold, which kept it disabled for signed-in users whose
personaId hadn't been hydrated.

Drop the !personaId guard from both modal and inline variants and
let the save go through — the POST /api/mycanvas/entries route
already resolves persona via getActivePersona() server-side, same
pattern as /api/community-content/generate.
```

## Body

With sign-in gating disabled, RemixDialog often runs without a
client-side personaId (identity flows server-side via the Bearer
token + spine). The Save button kept the !personaId guard from the
initial scaffold, which kept it disabled for signed-in users whose
personaId hadn't been hydrated.

Drop the !personaId guard from both modal and inline variants and
let the save go through — the POST /api/mycanvas/entries route
already resolves persona via getActivePersona() server-side, same
pattern as /api/community-content/generate.

## Files Changed

| Change | File |
|--------|------|
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 1 file changed, 5 insertions(+), 7 deletions(-)
