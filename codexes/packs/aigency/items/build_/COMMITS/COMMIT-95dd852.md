# Commit Brief: `95dd852` — add diagnostic logging for KnytTab paywall

| Field | Value |
|-------|-------|
| SHA | [`95dd852`](https://github.com/Kn0w-1/AigentZBeta/commit/95dd852924973dc77e8f45870fc82040e5e204fe) |
| Author | Claude |
| Date | 2026-05-15T05:03:52Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
add diagnostic logging for KnytTab paywall

Client (KnytTab.tsx):
- isEpisodeLocked: emit [KnytTab:LOCKED] console.warn the moment we hit the
  lock path, showing all decision inputs (effectivePersonaId, registryMapSize,
  ownedIssuesCount, ownedEpisodeNumbers list, hasAccessRestriction).
- Expose window.__knytDebug() to dump state on demand (effectivePersonaId,
  ownedIssues sample, ownedEpisodeNumbers, registry map, localStorage size).

Server (/api/codex/owned):
- Log every request: personaId, resolvedPersonaId, fioResolutionFailed,
  entitlementsCount, assetIds. Visible in Amplify CloudWatch.
- Log every response: issueCount, epNums, uploadedSlotKeysCount,
  expectedSlotsCount — so we can correlate API output with client-side
  paywall firing without needing browser logs.

To strip after root-cause is fixed.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

Client (KnytTab.tsx):
- isEpisodeLocked: emit [KnytTab:LOCKED] console.warn the moment we hit the
  lock path, showing all decision inputs (effectivePersonaId, registryMapSize,
  ownedIssuesCount, ownedEpisodeNumbers list, hasAccessRestriction).
- Expose window.__knytDebug() to dump state on demand (effectivePersonaId,
  ownedIssues sample, ownedEpisodeNumbers, registry map, localStorage size).

Server (/api/codex/owned):
- Log every request: personaId, resolvedPersonaId, fioResolutionFailed,
  entitlementsCount, assetIds. Visible in Amplify CloudWatch.
- Log every response: issueCount, epNums, uploadedSlotKeysCount,
  expectedSlotsCount — so we can correlate API output with client-side
  paywall firing without needing browser logs.

To strip after root-cause is fixed.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/codex/owned/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 67 insertions(+), 3 deletions(-)
