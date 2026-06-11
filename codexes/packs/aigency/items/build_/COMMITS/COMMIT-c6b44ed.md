# Commit Brief: `c6b44ed` — myCanvas: fix list 413 by lazy-loading body_md; surface save error

| Field | Value |
|-------|-------|
| SHA | [`c6b44ed`](https://github.com/Kn0w-1/AigentZBeta/commit/c6b44ed492802bb16e63b881967aca125abf1230) |
| Author | Claude |
| Date | 2026-05-22T06:51:09Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
myCanvas: fix list 413 by lazy-loading body_md; surface save error

GET /api/mycanvas/entries was returning every entry's full body_md.
With derived experience entries storing 600–900-word article bodies,
the cumulative response exceeded AWS Lambda's 6 MB payload limit and
the route started failing with 413 once a user had a handful of
saved remixes. That broke the whole tab — list never loaded, no
Publish, no Share, no anything.

Fix: split the read path.
  • listEntries now selects only metadata columns (id, title, tags,
    visibility, entry_type, meta_json, timestamps). body_md is
    omitted; entries arrive with bodyMd: ''.
  • New GET /api/mycanvas/entries/[id] returns the full row including
    body_md. canvasService gains getEntry() backing it.
  • MyCanvasTab hydrates the full body on first selection of any
    entry whose bodyMd is empty, and merges the response back into
    the list state so subsequent re-selects don't re-fetch.

RemixDialog.saveToCanvas now reads the failing response body and
surfaces { status, error } from the server instead of the generic
'Couldn't save to myCanvas' that hid the real reason. Console gets
a structured log line too for operator debug.
```

## Body

GET /api/mycanvas/entries was returning every entry's full body_md.
With derived experience entries storing 600–900-word article bodies,
the cumulative response exceeded AWS Lambda's 6 MB payload limit and
the route started failing with 413 once a user had a handful of
saved remixes. That broke the whole tab — list never loaded, no
Publish, no Share, no anything.

Fix: split the read path.
  • listEntries now selects only metadata columns (id, title, tags,
    visibility, entry_type, meta_json, timestamps). body_md is
    omitted; entries arrive with bodyMd: ''.
  • New GET /api/mycanvas/entries/[id] returns the full row including
    body_md. canvasService gains getEntry() backing it.
  • MyCanvasTab hydrates the full body on first selection of any
    entry whose bodyMd is empty, and merges the response back into
    the list state so subsequent re-selects don't re-fetch.

RemixDialog.saveToCanvas now reads the failing response body and
surfaces { status, error } from the server instead of the generic
'Couldn't save to myCanvas' that hid the real reason. Console gets
a structured log line too for operator debug.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/mycanvas/entries/[id]/route.ts` |
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |
| Modified | `services/mycanvas/canvasService.ts` |

## Stats

 4 files changed, 61 insertions(+), 5 deletions(-)
