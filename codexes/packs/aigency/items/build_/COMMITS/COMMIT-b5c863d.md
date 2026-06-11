# Commit Brief: `b5c863d` — feat(content-qube): Phase 7 — editions ledger seeding + common rarity (streaming-access)

| Field | Value |
|-------|-------|
| SHA | [`b5c863d`](https://github.com/Kn0w-1/AigentZBeta/commit/b5c863ddd740394dd78c3e84b9c3c35c3a178d23) |
| Author | Claude |
| Date | 2026-05-13T20:44:29Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
feat(content-qube): Phase 7 — editions ledger seeding + common rarity (streaming-access)

Two rarity classes with different lifecycle semantics:

  CANONICAL-MINTABLE (limited, pre-seeded, Base-mint eligible):
    legendary 18, epic 186, rare 1,654, secret_black_rare 2 → 1,860 / qube
  STREAMING-ACCESS (commons, unlimited, appended on sale):
    Encrypted + token-gated; shares canonical cyphertext (no per-holder mint);
    base_token_id / chain_minted_at remain NULL forever; ledger row still
    written per sale for audit/revenue.

Changes:
- supabase/migrations/20260513040000_content_qube_editions_seed.sql
  - ALTER rarity CHECK to allow 'common'
  - CREATE INDEX idx_cq_edition_common_seq (partial, MAX-lookup helper)
  - REPLACE v_content_qube_registry view to expose common_count
  - Seed 1,860 canonical editions per metaKnyts content_qube via
    generate_series + NOT EXISTS guard (idempotent)
- types/contentQube.ts
  - Add 'common' to ContentQubeRarity union
  - Add ContentQubeCanonicalRarity helper + isCanonicalRarity guard
  - Note CONTENT_QUBE_RARITY_COUNTS describes canonical subset only
- services/content/buildDisplayManifest.ts
  - RegistryViewRow gains common_count
  - rarity_breakdown.common = { total: row.common_count, issued: row.common_count }
    (commons exist only when sold; total == issued by definition)
  - rarity_counts surfaces canonical-mintable seed; commons via breakdown

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

Two rarity classes with different lifecycle semantics:

  CANONICAL-MINTABLE (limited, pre-seeded, Base-mint eligible):
    legendary 18, epic 186, rare 1,654, secret_black_rare 2 → 1,860 / qube
  STREAMING-ACCESS (commons, unlimited, appended on sale):
    Encrypted + token-gated; shares canonical cyphertext (no per-holder mint);
    base_token_id / chain_minted_at remain NULL forever; ledger row still
    written per sale for audit/revenue.

Changes:
- supabase/migrations/20260513040000_content_qube_editions_seed.sql
  - ALTER rarity CHECK to allow 'common'
  - CREATE INDEX idx_cq_edition_common_seq (partial, MAX-lookup helper)
  - REPLACE v_content_qube_registry view to expose common_count
  - Seed 1,860 canonical editions per metaKnyts content_qube via
    generate_series + NOT EXISTS guard (idempotent)
- types/contentQube.ts
  - Add 'common' to ContentQubeRarity union
  - Add ContentQubeCanonicalRarity helper + isCanonicalRarity guard
  - Note CONTENT_QUBE_RARITY_COUNTS describes canonical subset only
- services/content/buildDisplayManifest.ts
  - RegistryViewRow gains common_count
  - rarity_breakdown.common = { total: row.common_count, issued: row.common_count }
    (commons exist only when sold; total == issued by definition)
  - rarity_counts surfaces canonical-mintable seed; commons via breakdown

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `services/content/buildDisplayManifest.ts` |
| Added | `supabase/migrations/20260513040000_content_qube_editions_seed.sql` |
| Modified | `types/contentQube.ts` |

## Stats

 4 files changed, 195 insertions(+), 3 deletions(-)
