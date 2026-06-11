# Commit Brief: `82088b6` — Phase 9: ContentQube edition claim service + HTTP endpoint

| Field | Value |
|-------|-------|
| SHA | [`82088b6`](https://github.com/Kn0w-1/AigentZBeta/commit/82088b6196a94af0f39c8fbdce9e5df5d55ade9d) |
| Author | Claude |
| Date | 2026-05-13T21:15:05Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Phase 9: ContentQube edition claim service + HTTP endpoint

- services/content/claimEdition.ts: claimEditionForPurchase routes by rarity
  - canonical (legendary/epic/rare/secret_black_rare): claims lowest unissued
    edition_number for the qube+rarity via atomic UPDATE...WHERE persona_id
    IS NULL (with retry on race); idempotent per (qube, rarity, persona)
  - common: appends new row at MAX(edition_number)+1; retries on unique
    constraint violation (parallel insert race)
  - returns soldOut=true when canonical supply for the rarity is exhausted
- services/access/contentQubeReceiptEmitter.ts: add emitContentQubeTransferReceipt
  (receipt_kind='transfer', T2-safe, includes edition_id/edition_number/rarity
  and optional source_purchase_id)
- app/api/registry/content-qube/[id]/claim/route.ts: POST endpoint validates
  rarity, requires authenticated persona via spine, returns edition info or
  409 sold_out

Phase 9.1 (deferred): pipe evaluateAccess for proper aliasCommitment on the
transfer receipt; currently writes t2_alias_commitment=null for system-level
claims, which matches the pattern in emitContentQubeCreationReceipt.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

- services/content/claimEdition.ts: claimEditionForPurchase routes by rarity
  - canonical (legendary/epic/rare/secret_black_rare): claims lowest unissued
    edition_number for the qube+rarity via atomic UPDATE...WHERE persona_id
    IS NULL (with retry on race); idempotent per (qube, rarity, persona)
  - common: appends new row at MAX(edition_number)+1; retries on unique
    constraint violation (parallel insert race)
  - returns soldOut=true when canonical supply for the rarity is exhausted
- services/access/contentQubeReceiptEmitter.ts: add emitContentQubeTransferReceipt
  (receipt_kind='transfer', T2-safe, includes edition_id/edition_number/rarity
  and optional source_purchase_id)
- app/api/registry/content-qube/[id]/claim/route.ts: POST endpoint validates
  rarity, requires authenticated persona via spine, returns edition info or
  409 sold_out

Phase 9.1 (deferred): pipe evaluateAccess for proper aliasCommitment on the
transfer receipt; currently writes t2_alias_commitment=null for system-level
claims, which matches the pattern in emitContentQubeCreationReceipt.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/registry/content-qube/[id]/claim/route.ts` |
| Modified | `services/access/contentQubeReceiptEmitter.ts` |
| Added | `services/content/claimEdition.ts` |

## Stats

 3 files changed, 382 insertions(+)
