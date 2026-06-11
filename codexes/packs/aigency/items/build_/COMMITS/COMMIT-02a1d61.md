# Commit Brief: `02a1d61` — pipe aliasCommitment into edition claim + wire purchaseHandler to claimEditionForPurchase

| Field | Value |
|-------|-------|
| SHA | [`02a1d61`](https://github.com/Kn0w-1/AigentZBeta/commit/02a1d61212b970aec6032a5cde0837c6ac0df2a9) |
| Author | Claude |
| Date | 2026-05-13T21:23:17Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pipe aliasCommitment into edition claim + wire purchaseHandler to claimEditionForPurchase

Phase 9.1: claim route computes T2 alias commitment via cohortAliasService
  (same buildReceiptHandle logic as evaluateAccess) instead of hardcoded null;
  falls back to null if alias service unconfigured.

Phase 9.2: purchaseHandler.processPurchase now calls claimContentQubeEditions
  after entitlement grant — looks up content_qubes rows matching the granted
  assetIds (via master_qube_id / media_asset_id) and fires claimEditionForPurchase
  for each. Fire-and-forget tolerant; aliasCommitment derived from personaId
  + default cohort + current epoch.
```

## Body

Phase 9.1: claim route computes T2 alias commitment via cohortAliasService
  (same buildReceiptHandle logic as evaluateAccess) instead of hardcoded null;
  falls back to null if alias service unconfigured.

Phase 9.2: purchaseHandler.processPurchase now calls claimContentQubeEditions
  after entitlement grant — looks up content_qubes rows matching the granted
  assetIds (via master_qube_id / media_asset_id) and fires claimEditionForPurchase
  for each. Fire-and-forget tolerant; aliasCommitment derived from personaId
  + default cohort + current epoch.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/registry/content-qube/[id]/claim/route.ts` |
| Modified | `services/rewards/purchaseHandler.ts` |

## Stats

 2 files changed, 98 insertions(+), 5 deletions(-)
