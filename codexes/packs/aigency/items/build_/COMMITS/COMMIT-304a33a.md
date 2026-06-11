# Commit Brief: `304a33a` — link .md inline-code refs to GitHub in pack browser

| Field | Value |
|-------|-------|
| SHA | [`304a33a`](https://github.com/Kn0w-1/AigentZBeta/commit/304a33af33294b07e0949b41bb3803c7f5c44ec9) |
| Author | Claude |
| Date | 2026-05-21T22:03:49Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
link .md inline-code refs to GitHub in pack browser

Any inline-code token ending in .md now renders as an <a> link to the
canonical GitHub URL (iqube-protocol/aigentzbeta, dev branch).  Known
items use their registry path; unknown refs fall back to items/<basename>.
Replaces the previous in-app button navigation.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

Any inline-code token ending in .md now renders as an <a> link to the
canonical GitHub URL (iqube-protocol/aigentzbeta, dev branch).  Known
items use their registry path; unknown refs fall back to items/<basename>.
Replaces the previous in-app button navigation.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/PackBrowserTab.tsx` |

## Stats

 1 file changed, 13 insertions(+), 10 deletions(-)
