# Commit Brief: `9b0b69e` — fix(sql): store_skus.episode_numbers — convert public bundles to 0-indexed

| Field | Value |
|-------|-------|
| SHA | [`9b0b69e`](https://github.com/Kn0w-1/AigentZBeta/commit/9b0b69e4662dd3c228b3e372038ee412bb3ad7b0) |
| Author | Claude |
| Date | 2026-05-16T01:37:37Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix(sql): store_skus.episode_numbers — convert public bundles to 0-indexed

The May-11 SKU seed wrote public bundle episode_numbers as 1-indexed
arrays (legacy 'DB ep = display + 1' convention). The DB now uses
the canonical 0-indexed convention (master_content_qubes.episode_number
0..12, per /api/admin/codex/canonical).

skuCoversAsset compares sku.episode_numbers to AssetMeta.episodeNumber
by literal equality, so the off-by-one drops display #0 from every
public-bundle owner's grants and wastes a slot on the non-existent
display #13.

bundle-0-2:   [1,2,3]     → [0,1,2]
bundle-3-7:   [4,5,6,7,8] → [3,4,5,6,7]
bundle-8-12:  [9..13]     → [8..12]
bundle-full:  [1..13]     → [0..12]

Adds migration 20260516010000_store_skus_episode_numbers_zero_indexed.sql
to normalise existing rows, and updates the seed file so fresh
environments stay aligned. Operator must apply the new migration in
Supabase SQL editor (or via the deploy pipeline if migrations are
auto-run) for the change to take effect on dev-beta.
```

## Body

The May-11 SKU seed wrote public bundle episode_numbers as 1-indexed
arrays (legacy 'DB ep = display + 1' convention). The DB now uses
the canonical 0-indexed convention (master_content_qubes.episode_number
0..12, per /api/admin/codex/canonical).

skuCoversAsset compares sku.episode_numbers to AssetMeta.episodeNumber
by literal equality, so the off-by-one drops display #0 from every
public-bundle owner's grants and wastes a slot on the non-existent
display #13.

bundle-0-2:   [1,2,3]     → [0,1,2]
bundle-3-7:   [4,5,6,7,8] → [3,4,5,6,7]
bundle-8-12:  [9..13]     → [8..12]
bundle-full:  [1..13]     → [0..12]

Adds migration 20260516010000_store_skus_episode_numbers_zero_indexed.sql
to normalise existing rows, and updates the seed file so fresh
environments stay aligned. Operator must apply the new migration in
Supabase SQL editor (or via the deploy pipeline if migrations are
auto-run) for the change to take effect on dev-beta.

## Files Changed

| Change | File |
|--------|------|
| Modified | `supabase/migrations/20260511000000_store_skus_seed.sql` |
| Added | `supabase/migrations/20260516010000_store_skus_episode_numbers_zero_indexed.sql` |

## Stats

 2 files changed, 40 insertions(+), 5 deletions(-)
