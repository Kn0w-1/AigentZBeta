# Commit Brief: `c1dbf3c` — Add admin browser for ContentQube registry + fix bridge migration

| Field | Value |
|-------|-------|
| SHA | [`c1dbf3c`](https://github.com/Kn0w-1/AigentZBeta/commit/c1dbf3c9d6ae25ad75eb3d10f73061be44a503ee) |
| Author | Claude |
| Date | 2026-05-14T19:30:48Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Add admin browser for ContentQube registry + fix bridge migration

Browser surface so the operator can verify the ContentQube registry as
SoT independent of any codex tab plumbing:
- GET /api/registry/content-qube/browse (admin-gated)
  Lists v_content_qube_registry rows with T1-safe fields + a summary
  (counts by series / content_type / lifecycle_state). Storage URLs are
  intentionally omitted from the response per CLAUDE.md gated-content rule.
- /registry/content-qubes page (admin-gated, reads the browse endpoint)
  Plain table view with series + content_kind filters, summary cards,
  lifecycle badges. Shows the explicit 'migration not run' message when
  the metaKnyts filter returns zero rows.

Migration source fix:
- supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql
  - Added ::text casts on master_qube_id / media_asset_id JOIN conditions
    (text=uuid was failing under strict cross-type comparison)
  - Removed gating_kind reads from master_content_qubes / codex_media_assets
    (column never existed on either table); access policies default to
    'owned' for every metaKnyts row, since these are paid comics gated
    via SKU entitlements
  - Both fixes are idempotent via ON CONFLICT DO NOTHING

The current dev DB has the schema (010) + view (020) applied, but the
KNYT pilot bridge (030) rolled back on the type+column errors. The
corrected SQL is ready to paste into the Supabase editor; this commit
also fixes the source file so future supabase-db-push runs use the
corrected version.
```

## Body

Browser surface so the operator can verify the ContentQube registry as
SoT independent of any codex tab plumbing:
- GET /api/registry/content-qube/browse (admin-gated)
  Lists v_content_qube_registry rows with T1-safe fields + a summary
  (counts by series / content_type / lifecycle_state). Storage URLs are
  intentionally omitted from the response per CLAUDE.md gated-content rule.
- /registry/content-qubes page (admin-gated, reads the browse endpoint)
  Plain table view with series + content_kind filters, summary cards,
  lifecycle badges. Shows the explicit 'migration not run' message when
  the metaKnyts filter returns zero rows.

Migration source fix:
- supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql
  - Added ::text casts on master_qube_id / media_asset_id JOIN conditions
    (text=uuid was failing under strict cross-type comparison)
  - Removed gating_kind reads from master_content_qubes / codex_media_assets
    (column never existed on either table); access policies default to
    'owned' for every metaKnyts row, since these are paid comics gated
    via SKU entitlements
  - Both fixes are idempotent via ON CONFLICT DO NOTHING

The current dev DB has the schema (010) + view (020) applied, but the
KNYT pilot bridge (030) rolled back on the type+column errors. The
corrected SQL is ready to paste into the Supabase editor; this commit
also fixes the source file so future supabase-db-push runs use the
corrected version.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Added | `app/(shell)/registry/content-qubes/page.tsx` |
| Added | `app/api/registry/content-qube/browse/route.ts` |
| Modified | `supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql` |

## Stats

 4 files changed, 443 insertions(+), 29 deletions(-)
