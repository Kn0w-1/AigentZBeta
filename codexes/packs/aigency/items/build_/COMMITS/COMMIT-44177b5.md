# Commit Brief: `44177b5` — Fix KnytTab overlay: registry can only unlock, never lock

| Field | Value |
|-------|-------|
| SHA | [`44177b5`](https://github.com/Kn0w-1/AigentZBeta/commit/44177b5fda5ab78c9729bcb08fa539f114fb3050) |
| Author | Claude |
| Date | 2026-05-14T22:10:23Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Fix KnytTab overlay: registry can only unlock, never lock

Phase B introduced a registry-PRIMARY overlay in isEpisodeLocked and
openEpisodeVideo that called `!registryOwnership.get(key)` whenever the
key existed. That meant a `false` value in the registry map (stale module
cache, initial empty load, or a key the persona-owns probe hadn't resolved
yet) returned LOCKED and short-circuited the legacy variant-aware fallback.

This is the root cause of 'Owned badge shows but click routes to paywall'
on the Codex tab specifically. Shelf and Store don't use this overlay,
which is why they kept working even when Codex did not.

Fix: invert the semantics. Registry overlay can only CONFIRM ownership
(persona_owns === true → unlock). Anything else (false, missing key, hook
still loading) falls through to the legacy ownedIssues check, which is
already SKU-aware and contentTypes-aware after Phase A.

Same fix applied to openEpisodeVideo for the Watch quick-action.

Verified:
- /api/codex/owned returns 14 issues + 13 characters for arkagent@knyt
- /series-rights returns qubes[0].manifest.persona_owns: true
- Shelf shows 40 owned tiles correctly
- Store shows owned badges including ep 12 correctly
- Only Codex was broken; this fix unblocks it.
```

## Body

Phase B introduced a registry-PRIMARY overlay in isEpisodeLocked and
openEpisodeVideo that called `!registryOwnership.get(key)` whenever the
key existed. That meant a `false` value in the registry map (stale module
cache, initial empty load, or a key the persona-owns probe hadn't resolved
yet) returned LOCKED and short-circuited the legacy variant-aware fallback.

This is the root cause of 'Owned badge shows but click routes to paywall'
on the Codex tab specifically. Shelf and Store don't use this overlay,
which is why they kept working even when Codex did not.

Fix: invert the semantics. Registry overlay can only CONFIRM ownership
(persona_owns === true → unlock). Anything else (false, missing key, hook
still loading) falls through to the legacy ownedIssues check, which is
already SKU-aware and contentTypes-aware after Phase A.

Same fix applied to openEpisodeVideo for the Watch quick-action.

Verified:
- /api/codex/owned returns 14 issues + 13 characters for arkagent@knyt
- /series-rights returns qubes[0].manifest.persona_owns: true
- Shelf shows 40 owned tiles correctly
- Store shows owned badges including ep 12 correctly
- Only Codex was broken; this fix unblocks it.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 29 insertions(+), 23 deletions(-)
