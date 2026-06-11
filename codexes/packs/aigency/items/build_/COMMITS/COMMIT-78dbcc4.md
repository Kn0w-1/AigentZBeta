# Commit Brief: `78dbcc4` — Scrolls tab: derive isOwned from registry first, fall back to legacy

| Field | Value |
|-------|-------|
| SHA | [`78dbcc4`](https://github.com/Kn0w-1/AigentZBeta/commit/78dbcc4f4bf4e4649ed7ffa9428de72304510f7e) |
| Author | Claude |
| Date | 2026-05-14T23:23:59Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Scrolls tab: derive isOwned from registry first, fall back to legacy

Root cause of 'all episode cards route to paywall on codex tab even though
shelf shows them owned': scrolls tab renders each episode card with
cardAccess.evaluate({ manualOwned: isOwned }) where isOwned was sourced
exclusively from ownedIssues (the /api/codex/owned fetch result). When that
fetch is empty in the iframe context — even transiently — every card falls
through to cartCtaTarget='purchase' and click routes to the paywall.

The shelf and store both work because they consume the registry hook
(useContentQubeSeriesRights) which already returns the correct persona_owns
flags. The scrolls tab inside KnytTab was the last surface still on the
legacy ownership path.

Phase B canonicalization pattern: registry can only UNLOCK, legacy is the
fallback. Mirrors isEpisodeLocked at line 2213-2219.
```

## Body

Root cause of 'all episode cards route to paywall on codex tab even though
shelf shows them owned': scrolls tab renders each episode card with
cardAccess.evaluate({ manualOwned: isOwned }) where isOwned was sourced
exclusively from ownedIssues (the /api/codex/owned fetch result). When that
fetch is empty in the iframe context — even transiently — every card falls
through to cartCtaTarget='purchase' and click routes to the paywall.

The shelf and store both work because they consume the registry hook
(useContentQubeSeriesRights) which already returns the correct persona_owns
flags. The scrolls tab inside KnytTab was the last surface still on the
legacy ownership path.

Phase B canonicalization pattern: registry can only UNLOCK, legacy is the
fallback. Mirrors isEpisodeLocked at line 2213-2219.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 10 insertions(+), 1 deletion(-)
