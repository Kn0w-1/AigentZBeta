# Commit Brief: `842f651` — myCanvas list 413: also strip meta_json from list response

| Field | Value |
|-------|-------|
| SHA | [`842f651`](https://github.com/Kn0w-1/AigentZBeta/commit/842f651a34d0183067a90897e6f77cd9d5372e91) |
| Author | Claude |
| Date | 2026-05-22T06:52:30Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
myCanvas list 413: also strip meta_json from list response

The previous fix stripped body_md but left meta_json. Turns out
generateImage() returns base64 data URLs (data:image/png;base64,...)
which can run 100KB–1MB+ each, and we copy that into
mycanvas_entries.meta_json.imageUrl on save. Cumulative meta_json
size across a handful of remixes still blew past the 6 MB Lambda
payload limit, keeping the 413 in place.

Strip meta_json from listEntries too — the sidebar doesn't need it
(it only renders title, entry_type icon, timestamps, visibility).
The full meta_json (with the base64 image) arrives via GET /[id]
when the user selects the entry. MyCanvasTab's hydration effect
now considers both body_md AND meta_json: triggers when bodyMd is
empty AND metaJson is {} so list-stripped entries always hydrate
on first selection.

Real fix is to upload images to storage and store https URLs in
the DB instead of base64 data URLs — that's a separate backlog
item. This unblocks the tab in the meantime.
```

## Body

The previous fix stripped body_md but left meta_json. Turns out
generateImage() returns base64 data URLs (data:image/png;base64,...)
which can run 100KB–1MB+ each, and we copy that into
mycanvas_entries.meta_json.imageUrl on save. Cumulative meta_json
size across a handful of remixes still blew past the 6 MB Lambda
payload limit, keeping the 413 in place.

Strip meta_json from listEntries too — the sidebar doesn't need it
(it only renders title, entry_type icon, timestamps, visibility).
The full meta_json (with the base64 image) arrives via GET /[id]
when the user selects the entry. MyCanvasTab's hydration effect
now considers both body_md AND meta_json: triggers when bodyMd is
empty AND metaJson is {} so list-stripped entries always hydrate
on first selection.

Real fix is to upload images to storage and store https URLs in
the DB instead of base64 data URLs — that's a separate backlog
item. This unblocks the tab in the meantime.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `services/mycanvas/canvasService.ts` |

## Stats

 2 files changed, 17 insertions(+), 11 deletions(-)
