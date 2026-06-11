# Commit Brief: `d90b446` — re-apply 413 fix surgically: strip body_md+meta_json from list, hydrate on select

| Field | Value |
|-------|-------|
| SHA | [`d90b446`](https://github.com/Kn0w-1/AigentZBeta/commit/d90b4466947fbefb0a72103a3958d830820d2e21) |
| Author | Claude |
| Date | 2026-05-22T07:52:20Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
re-apply 413 fix surgically: strip body_md+meta_json from list, hydrate on select

Root cause: derived experience entries store full article bodies in
body_md AND base64-encoded images (data:image/png;base64,...) in
meta_json.imageUrl. With a handful of saved remixes, the cumulative
list response exceeds AWS Lambda's 6 MB payload limit (413) and on
retry eventually 504s. After that the list silently returns empty.

The fix is split into five clearly-annotated pieces, all in one commit
because the server strip and the client hydration are inseparable
(splitting would leave the editor showing empty bodies and overwriting
real data on save):

  PIECE 1  services/mycanvas/canvasService.ts::listEntries
           SELECT only metadata columns (id, title, tags, visibility,
           entry_type, timestamps). body_md + meta_json are omitted.

  PIECE 2  services/mycanvas/canvasService.ts::getEntry (new)
           Full-row fetcher backing the new GET endpoint. One entry is
           well under 6 MB even with a ~1 MB base64 image attached.

  PIECE 3  app/api/mycanvas/entries/[id]/route.ts::GET (new)
           Returns the full entry on demand. Auth via getActivePersona,
           same persona scoping as PATCH/DELETE.

  PIECE 4  MyCanvasTab.tsx::hydratedRef
           useRef<Set> tracking which IDs have been hydrated. Without
           this, note entries with legitimate empty body+meta retrigger
           the effect on every setEntries (the GET response equals the
           list shape → object reference changes → effect re-fires →
           infinite GET loop). The ref clears on personaId change.

  PIECE 5  MyCanvasTab.tsx — hydration effect
           On entry select, fetch full row via GET /[id] and merge body
           + meta back into list state. Each entry fetched at most once
           per persona. Transient errors un-mark for retry on next click.

What this fix deliberately does NOT include vs the original attempt:
  - No stale-entry 404 cleanup (was extra surface area)
  - No save-error message improvements in RemixDialog (unrelated)

Real fix is to upload generated images to Supabase storage and persist
HTTPS URLs instead of base64 data URLs — that's a separate backlog item.
This unblocks the tab in the meantime.
```

## Body

Root cause: derived experience entries store full article bodies in
body_md AND base64-encoded images (data:image/png;base64,...) in
meta_json.imageUrl. With a handful of saved remixes, the cumulative
list response exceeds AWS Lambda's 6 MB payload limit (413) and on
retry eventually 504s. After that the list silently returns empty.

The fix is split into five clearly-annotated pieces, all in one commit
because the server strip and the client hydration are inseparable
(splitting would leave the editor showing empty bodies and overwriting
real data on save):

  PIECE 1  services/mycanvas/canvasService.ts::listEntries
           SELECT only metadata columns (id, title, tags, visibility,
           entry_type, timestamps). body_md + meta_json are omitted.

  PIECE 2  services/mycanvas/canvasService.ts::getEntry (new)
           Full-row fetcher backing the new GET endpoint. One entry is
           well under 6 MB even with a ~1 MB base64 image attached.

  PIECE 3  app/api/mycanvas/entries/[id]/route.ts::GET (new)
           Returns the full entry on demand. Auth via getActivePersona,
           same persona scoping as PATCH/DELETE.

  PIECE 4  MyCanvasTab.tsx::hydratedRef
           useRef<Set> tracking which IDs have been hydrated. Without
           this, note entries with legitimate empty body+meta retrigger
           the effect on every setEntries (the GET response equals the
           list shape → object reference changes → effect re-fires →
           infinite GET loop). The ref clears on personaId change.

  PIECE 5  MyCanvasTab.tsx — hydration effect
           On entry select, fetch full row via GET /[id] and merge body
           + meta back into list state. Each entry fetched at most once
           per persona. Transient errors un-mark for retry on next click.

What this fix deliberately does NOT include vs the original attempt:
  - No stale-entry 404 cleanup (was extra surface area)
  - No save-error message improvements in RemixDialog (unrelated)

Real fix is to upload generated images to Supabase storage and persist
HTTPS URLs instead of base64 data URLs — that's a separate backlog item.
This unblocks the tab in the meantime.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/mycanvas/entries/[id]/route.ts` |
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `services/mycanvas/canvasService.ts` |

## Stats

 3 files changed, 80 insertions(+), 4 deletions(-)
