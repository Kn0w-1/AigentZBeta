# Commit Brief: `9e899d7` — community detail: lazy-load full articleBody via GET /[id] (list strip left only prompt)

| Field | Value |
|-------|-------|
| SHA | [`9e899d7`](https://github.com/Kn0w-1/AigentZBeta/commit/9e899d798affbf03fdd3adf45808cde073af826d) |
| Author | Claude |
| Date | 2026-05-23T08:36:51Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
community detail: lazy-load full articleBody via GET /[id] (list strip left only prompt)

After stripping articleBody from /api/community-content/list to avoid
the 6MB Lambda 413, the detail view fell back to 'item.articleBody ||
item.prompt' — so users saw only the short prompt instead of the full
600-900 word article. The /api/community-content/[id] endpoint already
returns the full row; just hydrate when the detail opens.

ContentDetail now:
  • Seeds fullBody state from the (null) list value
  • useEffect fetches GET /[id] when fullBody is null
  • Shows 'Loading full article…' italic placeholder during fetch
  • Renders fullBody once arrived, falling back to prompt only on
    fetch error / 404
```

## Body

After stripping articleBody from /api/community-content/list to avoid
the 6MB Lambda 413, the detail view fell back to 'item.articleBody ||
item.prompt' — so users saw only the short prompt instead of the full
600-900 word article. The /api/community-content/[id] endpoint already
returns the full row; just hydrate when the detail opens.

ContentDetail now:
  • Seeds fullBody state from the (null) list value
  • useEffect fetches GET /[id] when fullBody is null
  • Shows 'Loading full article…' italic placeholder during fetch
  • Renders fullBody once arrived, falling back to prompt only on
    fetch error / 404

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytCommunityContentTab.tsx` |

## Stats

 1 file changed, 25 insertions(+), 1 deletion(-)
