# Commit Brief: `576dfaa` — Phase A: variant-aware ownership gates in KnytTab (fix owned→payment-modal)

| Field | Value |
|-------|-------|
| SHA | [`576dfaa`](https://github.com/Kn0w-1/AigentZBeta/commit/576dfaa69c50ae20334e2d9c3e1725162f4cc2e7) |
| Author | Claude |
| Date | 2026-05-14T18:09:27Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Phase A: variant-aware ownership gates in KnytTab (fix owned→payment-modal)

Root cause: `OwnedIssue` from /api/codex/owned was variant-blind (keyed by
episodeNumber only). `KnytTab.isEpisodeLocked` and `openEpisodeVideo` both
treated 'persona owns ep12 print' the same as 'persona owns ep12 motion',
so clicking the motion variant passed the lock check, then the downstream
delivery gate (`userOwnsAsset(personaId, 'mk_ep12_motion')`) refused and
the payment modal fired despite the Owned badge.

Server fix:
- Add contentTypes[]: string[] to OwnedIssue (route.ts)
- Populate from the per-slot accumulator already computed internally;
  merge available + coming-soon formats so consumers see the full
  rights-grid for each episode

Client fix:
- Add contentTypes/comingSoon/owned to OwnedIssueFromAPI
- New resolveVariant() helper maps KnytContentItem.type to canonical
  variant ('episode_still' | 'episode_motion' | 'episode_print')
- isEpisodeLocked filters ownedIssues on BOTH episodeNumber AND
  contentTypes.includes(variant), with a legacy fallback for pre-rollout
  responses (no contentTypes field)
- openEpisodeVideo (motion player) requires 'episode_motion' in contentTypes
  specifically — defence-in-depth against unowned motion playback

Phase B (next commits) will canonicalize this on the ContentQube registry.
```

## Body

Root cause: `OwnedIssue` from /api/codex/owned was variant-blind (keyed by
episodeNumber only). `KnytTab.isEpisodeLocked` and `openEpisodeVideo` both
treated 'persona owns ep12 print' the same as 'persona owns ep12 motion',
so clicking the motion variant passed the lock check, then the downstream
delivery gate (`userOwnsAsset(personaId, 'mk_ep12_motion')`) refused and
the payment modal fired despite the Owned badge.

Server fix:
- Add contentTypes[]: string[] to OwnedIssue (route.ts)
- Populate from the per-slot accumulator already computed internally;
  merge available + coming-soon formats so consumers see the full
  rights-grid for each episode

Client fix:
- Add contentTypes/comingSoon/owned to OwnedIssueFromAPI
- New resolveVariant() helper maps KnytContentItem.type to canonical
  variant ('episode_still' | 'episode_motion' | 'episode_print')
- isEpisodeLocked filters ownedIssues on BOTH episodeNumber AND
  contentTypes.includes(variant), with a legacy fallback for pre-rollout
  responses (no contentTypes field)
- openEpisodeVideo (motion player) requires 'episode_motion' in contentTypes
  specifically — defence-in-depth against unowned motion playback

Phase B (next commits) will canonicalize this on the ContentQube registry.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/codex/owned/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 84 insertions(+), 15 deletions(-)
