# Commit Brief: `e855809` — Revert isEpisodeLocked to episode-number-only check (restore May 11 working logic)

| Field | Value |
|-------|-------|
| SHA | [`e855809`](https://github.com/Kn0w-1/AigentZBeta/commit/e8558097f2484b0bcab8aa9c9ba70ba0d7e10f29) |
| Author | Claude |
| Date | 2026-05-15T01:07:35Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Revert isEpisodeLocked to episode-number-only check (restore May 11 working logic)

The variant-aware contentTypes check introduced in Phase A (576dfaa6)
broke the gate in the opposite direction: any owned episode whose
contentTypes array didn't contain the clicked variant was locked even
though the persona owned it.

The working logic at b422c821 was:
  if (ownedForEp.length > 0) return false;

This was sufficient — if ANY owned issue exists for the episode, unlock.
Restoring this for both isEpisodeLocked and openEpisodeVideo, while
keeping the registry as a fast-path unlock-only signal.

scrolls-tab isOwned already has the dual-path fix (registry OR legacy)
from 78dbcc4f. This commit aligns isEpisodeLocked with the same
episode-number-only fallback so LiquidUI item clicks also unlock.
```

## Body

The variant-aware contentTypes check introduced in Phase A (576dfaa6)
broke the gate in the opposite direction: any owned episode whose
contentTypes array didn't contain the clicked variant was locked even
though the persona owned it.

The working logic at b422c821 was:
  if (ownedForEp.length > 0) return false;

This was sufficient — if ANY owned issue exists for the episode, unlock.
Restoring this for both isEpisodeLocked and openEpisodeVideo, while
keeping the registry as a fast-path unlock-only signal.

scrolls-tab isOwned already has the dual-path fix (registry OR legacy)
from 78dbcc4f. This commit aligns isEpisodeLocked with the same
episode-number-only fallback so LiquidUI item clicks also unlock.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 10 insertions(+), 39 deletions(-)
