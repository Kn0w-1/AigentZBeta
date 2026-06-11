# Commit Brief: `d9d3e7f` — restore: KnytTab gnEp capture at episode_number=-1 (brings card #3 back)

| Field | Value |
|-------|-------|
| SHA | [`d9d3e7f`](https://github.com/Kn0w-1/AigentZBeta/commit/d9d3e7f0dc23eb430aadadb9f893270c9b17e2db) |
| Author | Claude |
| Date | 2026-05-16T03:16:33Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
restore: KnytTab gnEp capture at episode_number=-1 (brings card #3 back)

Per operator: the 'Gen Zero Divided by One' card (display #0 with the
archer cover, sourced from DB ep 0) was the correct card. The duplicate
'Episode #0 / Episode #0' card with the motorcycle cover is the one to
remove — but it's not produced by the gnEp capture change. Investigation
of that duplicate is the next task.
```

## Body

Per operator: the 'Gen Zero Divided by One' card (display #0 with the
archer cover, sourced from DB ep 0) was the correct card. The duplicate
'Episode #0 / Episode #0' card with the motorcycle cover is the one to
remove — but it's not produced by the gnEp capture change. Investigation
of that duplicate is the next task.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 5 insertions(+), 5 deletions(-)
