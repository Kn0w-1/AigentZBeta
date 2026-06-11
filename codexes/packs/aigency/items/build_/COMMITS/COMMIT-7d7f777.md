# Commit Brief: `7d7f777` — fix owned-issues blank window: cache full OwnedIssueFromAPI[] and add ownedEpisodeNumbers fallback

| Field | Value |
|-------|-------|
| SHA | [`7d7f777`](https://github.com/Kn0w-1/AigentZBeta/commit/7d7f77758af9073e45f7d3e9430d131a77886c93) |
| Author | Claude |
| Date | 2026-05-15T02:56:09Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix owned-issues blank window: cache full OwnedIssueFromAPI[] and add ownedEpisodeNumbers fallback

- fetchOwnedEpisodes: cache key bumped to v2, now stores full OwnedIssueFromAPI[]
  so ownedIssues is pre-populated immediately on re-render instead of waiting for fetch
- isEpisodeLocked: fallback to ownedEpisodeNumbers when ownedIssues still empty (timing gap)
- getOwnedIssuesForEpisode: same fallback, returns synthetic minimal entry
- openEpisodeVideo: epNumsOwns check using ownedEpisodeNumbers as secondary gate
- explicit OwnedIssueFromAPI type on filter callbacks to satisfy TS strict mode

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

- fetchOwnedEpisodes: cache key bumped to v2, now stores full OwnedIssueFromAPI[]
  so ownedIssues is pre-populated immediately on re-render instead of waiting for fetch
- isEpisodeLocked: fallback to ownedEpisodeNumbers when ownedIssues still empty (timing gap)
- getOwnedIssuesForEpisode: same fallback, returns synthetic minimal entry
- openEpisodeVideo: epNumsOwns check using ownedEpisodeNumbers as secondary gate
- explicit OwnedIssueFromAPI type on filter callbacks to satisfy TS strict mode

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 43 insertions(+), 20 deletions(-)
