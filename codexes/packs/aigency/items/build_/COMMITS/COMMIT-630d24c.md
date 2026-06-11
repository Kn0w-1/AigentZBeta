# Commit Brief: `630d24c` — revert myCanvas to the build where remix-in-myCanvas was first enabled

| Field | Value |
|-------|-------|
| SHA | [`630d24c`](https://github.com/Kn0w-1/AigentZBeta/commit/630d24ce364c107f6f8287abb18652c3c8cf8a6b) |
| Author | Claude |
| Date | 2026-05-22T07:28:35Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
revert myCanvas to the build where remix-in-myCanvas was first enabled

Rolling back the three follow-on fix attempts that didn't land cleanly:

  c6b44ed4  myCanvas: fix list 413 by lazy-loading body_md
  842f651a  myCanvas list 413: also strip meta_json from list response
  af6e0bba  fix myCanvas hydration infinite loop + handle stale-entry 404

These touched only:
  - app/api/mycanvas/entries/[id]/route.ts
  - app/triad/components/codex/tabs/MyCanvasTab.tsx
  - components/metame/runtime/RemixDialog.tsx
  - services/mycanvas/canvasService.ts

All four are now back to the state at 5f309312 (the build where
remix/save/share/invite/publish from myCanvas first shipped). Other
work from this session (wallet drawer overlay fix, Be menu label,
runtime → shell persona broadcast wiring) is unchanged.

Next: re-apply the 413 fix surgically, in isolation, so we can
observe what specifically breaks the saved-entry-not-showing-up
behaviour.
```

## Body

Rolling back the three follow-on fix attempts that didn't land cleanly:

  c6b44ed4  myCanvas: fix list 413 by lazy-loading body_md
  842f651a  myCanvas list 413: also strip meta_json from list response
  af6e0bba  fix myCanvas hydration infinite loop + handle stale-entry 404

These touched only:
  - app/api/mycanvas/entries/[id]/route.ts
  - app/triad/components/codex/tabs/MyCanvasTab.tsx
  - components/metame/runtime/RemixDialog.tsx
  - services/mycanvas/canvasService.ts

All four are now back to the state at 5f309312 (the build where
remix/save/share/invite/publish from myCanvas first shipped). Other
work from this session (wallet drawer overlay fix, Be menu label,
runtime → shell persona broadcast wiring) is unchanged.

Next: re-apply the 413 fix surgically, in isolation, so we can
observe what specifically breaks the saved-entry-not-showing-up
behaviour.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/mycanvas/entries/[id]/route.ts` |
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |
| Modified | `services/mycanvas/canvasService.ts` |

## Stats

 4 files changed, 6 insertions(+), 94 deletions(-)
