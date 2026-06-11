# Commit Brief: `13cc056` — codex convention: anchor episode_number as the display number (0..12) with GN at -1

| Field | Value |
|-------|-------|
| SHA | [`13cc056`](https://github.com/Kn0w-1/AigentZBeta/commit/13cc0565b2c1b2d9fe1f8f8266c0df690bc7640f) |
| Author | Claude |
| Date | 2026-05-16T05:14:43Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
codex convention: anchor episode_number as the display number (0..12) with GN at -1

The episode-number convention was drifting between code and data:
the dev DB stores 13 metaKnyts episodes at master_content_qubes.episode_number
0..12 with GN at -1 (content_type=gn_still), but many consumers still applied
a -1 display offset and treated episode_number=0 as the GN. The two layers of
masking (admin renderEpisodeLabel subtracting 1, KnytTab gnEp capture at ep 0,
descriptor mapping ep 0 to 'gn', assetOwnership classifying ep 0 as 'gn',
status/import/upload-metadata routes deriving display as #(ep-1)) happened to
line up under the old SKU seed [1..13] but broke as soon as the SKU was
re-seeded to [0..12], surfacing the first real episode as "GN" in the admin
panel and shifting every subsequent label by -1.

Anchored every episode surface on the canonical convention documented in
supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql:14 and the
canonical-route's existing episode displayFormula:
  master_content_qubes.episode_number IS the display number (0..12)
  GN sits at episode_number = -1 with content_type = 'gn_still'
  -2..-4 reserved for legacy preorder rarity drops

Character convention (codex_media_assets 1-indexed: DB 1..13 = display #0..#12)
is independent and intentionally untouched — the character convention bridge
in api/codex/owned, api/codex/knyt-cards, the canonical route's character
block, and KnytTab.derivedCharacterGroups all keep their +/-1 translation.

Cache keys bumped to v9/v7/v7 so existing clients drop their old snapshots
and re-render against the new convention on next load.
```

## Body

The episode-number convention was drifting between code and data:
the dev DB stores 13 metaKnyts episodes at master_content_qubes.episode_number
0..12 with GN at -1 (content_type=gn_still), but many consumers still applied
a -1 display offset and treated episode_number=0 as the GN. The two layers of
masking (admin renderEpisodeLabel subtracting 1, KnytTab gnEp capture at ep 0,
descriptor mapping ep 0 to 'gn', assetOwnership classifying ep 0 as 'gn',
status/import/upload-metadata routes deriving display as #(ep-1)) happened to
line up under the old SKU seed [1..13] but broke as soon as the SKU was
re-seeded to [0..12], surfacing the first real episode as "GN" in the admin
panel and shifting every subsequent label by -1.

Anchored every episode surface on the canonical convention documented in
supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql:14 and the
canonical-route's existing episode displayFormula:
  master_content_qubes.episode_number IS the display number (0..12)
  GN sits at episode_number = -1 with content_type = 'gn_still'
  -2..-4 reserved for legacy preorder rarity drops

Character convention (codex_media_assets 1-indexed: DB 1..13 = display #0..#12)
is independent and intentionally untouched — the character convention bridge
in api/codex/owned, api/codex/knyt-cards, the canonical route's character
block, and KnytTab.derivedCharacterGroups all keep their +/-1 translation.

Cache keys bumped to v9/v7/v7 so existing clients drop their old snapshots
and re-render against the new convention on next load.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/(shell)/admin/codex/components/CodexUploadModal.tsx` |
| Modified | `app/api/admin/codex/canonical/route.ts` |
| Modified | `app/api/admin/codex/import/route.ts` |
| Modified | `app/api/admin/codex/status/route.ts` |
| Modified | `app/api/admin/codex/upload-metadata/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |
| Modified | `app/triad/components/codex/tabs/QriptopianAdminTab.tsx` |
| Modified | `app/triad/components/codex/tabs/ScrollsTab.tsx` |
| Modified | `services/content/getContentDescriptor.ts` |
| Modified | `services/rewards/assetOwnership.ts` |

## Stats

 10 files changed, 49 insertions(+), 34 deletions(-)
