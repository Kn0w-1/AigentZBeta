# Commit Brief: `28181dc` — fix KnytTab paywall: localStorage persistence + unconditional ownedEpisodeNumbers fallback

| Field | Value |
|-------|-------|
| SHA | [`28181dc`](https://github.com/Kn0w-1/AigentZBeta/commit/28181dc5b63369f6b516059c1e5a11baf2e1db45) |
| Author | Claude |
| Date | 2026-05-15T03:47:06Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix KnytTab paywall: localStorage persistence + unconditional ownedEpisodeNumbers fallback

- Add useEffect that reads ownedIssues from localStorage the moment
  effectivePersonaId resolves — eliminates the blank window on every page
  reload (in-memory cache only survived same-session navigation, not refresh)
- After successful /api/codex/owned fetch: persist freshIssues to localStorage
  so next page load has data immediately, before the async fetch completes
- Remove the ownedIssues.length === 0 guard from isEpisodeLocked and
  openEpisodeVideo — ownedEpisodeNumbers now fires unconditionally as a
  fallback (safe: it is always derived from the same data as ownedIssues)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

- Add useEffect that reads ownedIssues from localStorage the moment
  effectivePersonaId resolves — eliminates the blank window on every page
  reload (in-memory cache only survived same-session navigation, not refresh)
- After successful /api/codex/owned fetch: persist freshIssues to localStorage
  so next page load has data immediately, before the async fetch completes
- Remove the ownedIssues.length === 0 guard from isEpisodeLocked and
  openEpisodeVideo — ownedEpisodeNumbers now fires unconditionally as a
  fallback (safe: it is always derived from the same data as ownedIssues)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 36 insertions(+), 11 deletions(-)
