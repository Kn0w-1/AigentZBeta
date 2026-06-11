# Commit Brief: `20a7f03` — shell: render active persona display name as Be menu label

| Field | Value |
|-------|-------|
| SHA | [`20a7f03`](https://github.com/Kn0w-1/AigentZBeta/commit/20a7f03db59dfa2cbd5f98236958bcdcfa749947) |
| Author | Claude |
| Date | 2026-05-22T05:52:12Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
shell: render active persona display name as Be menu label

The platform Next.js shell now listens for metame:persona-changed (and
legacy aa-persona-change-v1) postMessage broadcasts from the runtime
iframe — these envelopes already carry { personaId, displayLabel,
ownFioHandle, surface } inline per the PersonaContext broadcast
contract, so no fetch from the shell is needed. The runtime owns the
spine integration; the shell just renders what it receives.

When a label arrives, it's passed to SmartMenu as beLabelOverride and
applied only to the menu item with id 'be' (so other actions are
unaffected if they ever share the override prop in future).

Label precedence matches PersonaContext: displayLabel from the surface
takes priority over ownFioHandle. If no broadcast has arrived yet, the
existing 'Be' fallback continues to render.

The same fix is needed in Lovable's thin client — instruction provided
separately.
```

## Body

The platform Next.js shell now listens for metame:persona-changed (and
legacy aa-persona-change-v1) postMessage broadcasts from the runtime
iframe — these envelopes already carry { personaId, displayLabel,
ownFioHandle, surface } inline per the PersonaContext broadcast
contract, so no fetch from the shell is needed. The runtime owns the
spine integration; the shell just renders what it receives.

When a label arrives, it's passed to SmartMenu as beLabelOverride and
applied only to the menu item with id 'be' (so other actions are
unaffected if they ever share the override prop in future).

Label precedence matches PersonaContext: displayLabel from the surface
takes priority over ownFioHandle. If no broadcast has arrived yet, the
existing 'Be' fallback continues to render.

The same fix is needed in Lovable's thin client — instruction provided
separately.

## Files Changed

| Change | File |
|--------|------|
| Modified | `apps/metame-runtime-shell/app/components/SmartMenu.tsx` |
| Modified | `apps/metame-runtime-shell/app/page.tsx` |

## Stats

 2 files changed, 63 insertions(+), 5 deletions(-)
