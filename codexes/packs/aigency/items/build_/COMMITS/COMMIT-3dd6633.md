# Commit Brief: `3dd6633` — Activations tab + myCanvas + Order of Metayé — persona-driven runtime activations

| Field | Value |
|-------|-------|
| SHA | [`3dd6633`](https://github.com/Kn0w-1/AigentZBeta/commit/3dd6633b1baea85c40fda4b9fa0319599fda8384) |
| Author | Claude |
| Date | 2026-05-19T20:19:57Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Activations tab + myCanvas + Order of Metayé — persona-driven runtime activations
```

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/admin/activations/grant/route.ts` |
| Added | `app/api/assistant/activations/[id]/route.ts` |
| Added | `app/api/assistant/activations/route.ts` |
| Added | `app/api/mycanvas/entries/[id]/invite/route.ts` |
| Added | `app/api/mycanvas/entries/[id]/route.ts` |
| Added | `app/api/mycanvas/entries/route.ts` |
| Modified | `app/hooks/useCodexConfig.ts` |
| Modified | `app/triad/components/CodexPanelDynamic.tsx` |
| Modified | `app/triad/components/codex/TabRenderer.tsx` |
| Added | `app/triad/components/codex/tabs/ActivationsTab.tsx` |
| Added | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Added | `data/activation-catalog.ts` |
| Modified | `data/codex-configs.ts` |
| Added | `services/activations/personaActivations.ts` |
| Added | `services/mycanvas/canvasService.ts` |
| Added | `supabase/migrations/20260523000000_persona_activations.sql` |
| Added | `supabase/migrations/20260523010000_mycanvas.sql` |
| Modified | `types/codex.ts` |

## Stats

 18 files changed, 1755 insertions(+), 29 deletions(-)
