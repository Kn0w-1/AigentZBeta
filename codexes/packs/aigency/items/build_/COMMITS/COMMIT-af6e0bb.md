# Commit Brief: `af6e0bb` — fix myCanvas hydration infinite loop + handle stale-entry 404

| Field | Value |
|-------|-------|
| SHA | [`af6e0bb`](https://github.com/Kn0w-1/AigentZBeta/commit/af6e0bbafee163674ced74b0dc39356dd6910b1b) |
| Author | Claude |
| Date | 2026-05-22T07:09:47Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix myCanvas hydration infinite loop + handle stale-entry 404

Regression from the previous list-strip change: the hydration effect
fired for any entry whose bodyMd was '' AND metaJson was {}. For notes
(which legitimately have both empty) the GET response equals the list
shape, setEntries replaces the entry object, the 'selected' ref
changes, the effect re-fires, fetch again — infinite loop hammering
GET /api/mycanvas/entries/[id].

Fix: track hydrated entry IDs in a useRef Set. The effect fetches at
most once per entry per persona. The set is cleared on personaId
change so a freshly-stripped list re-hydrates on next click. On
transient (non-404) errors the ID is un-marked so the next selection
gets another shot.

While here: handle 404 on the detail fetch as a stale-entry signal.
When the list still contains an entry that the server rejects (e.g.
created under a different persona, or deleted in another session),
drop it from local state and clear selectedId so the right panel
doesn't render a broken view.
```

## Body

Regression from the previous list-strip change: the hydration effect
fired for any entry whose bodyMd was '' AND metaJson was {}. For notes
(which legitimately have both empty) the GET response equals the list
shape, setEntries replaces the entry object, the 'selected' ref
changes, the effect re-fires, fetch again — infinite loop hammering
GET /api/mycanvas/entries/[id].

Fix: track hydrated entry IDs in a useRef Set. The effect fetches at
most once per entry per persona. The set is cleared on personaId
change so a freshly-stripped list re-hydrates on next click. On
transient (non-404) errors the ID is un-marked so the next selection
gets another shot.

While here: handle 404 on the detail fetch as a stale-entry signal.
When the list still contains an entry that the server rejects (e.g.
created under a different persona, or deleted in another session),
drop it from local state and clear selectedId so the right panel
doesn't render a broken view.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |

## Stats

 1 file changed, 34 insertions(+), 8 deletions(-)
