# Commit Brief: `897314a` — feat(content-qube): Phase 6 — KNYT pilot bridge migration

| Field | Value |
|-------|-------|
| SHA | [`897314a`](https://github.com/Kn0w-1/AigentZBeta/commit/897314a49dbe8a391395dd51f963cb9aa4e8cf9c) |
| Author | Claude |
| Date | 2026-05-13T20:26:28Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
feat(content-qube): Phase 6 — KNYT pilot bridge migration

supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql
  Idempotent migration that seeds content_qubes for all metaKnyts rows:
  1. Inserts from master_content_qubes (episodes + GN) with master_qube_id
  2. Inserts from codex_media_assets (characters + powers_sheets) with
     media_asset_id; display_number = episode_number - 1 (1→0 bridge)
  3. Copies primary CIDs into content_qube_storage (storage_kind derived
     from auto_drive_cid shape: http% → supabase, else → auto_drive)
  4. Seeds content_qube_access_policies from existing gating_kind columns
     (payment → owned, credential → subscription, free/null → free)
  5. Binds all qubes to knyt-codex (episodes/GN → scrolls tab, characters
     → characters tab) via content_qube_cartridge_bindings
  6. Emits one system-level creation DVN receipt per qube (no persona
     attribution; skips if receipt already exists — idempotent)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql
  Idempotent migration that seeds content_qubes for all metaKnyts rows:
  1. Inserts from master_content_qubes (episodes + GN) with master_qube_id
  2. Inserts from codex_media_assets (characters + powers_sheets) with
     media_asset_id; display_number = episode_number - 1 (1→0 bridge)
  3. Copies primary CIDs into content_qube_storage (storage_kind derived
     from auto_drive_cid shape: http% → supabase, else → auto_drive)
  4. Seeds content_qube_access_policies from existing gating_kind columns
     (payment → owned, credential → subscription, free/null → free)
  5. Binds all qubes to knyt-codex (episodes/GN → scrolls tab, characters
     → characters tab) via content_qube_cartridge_bindings
  6. Emits one system-level creation DVN receipt per qube (no persona
     attribution; skips if receipt already exists — idempotent)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Added | `supabase/migrations/20260513030000_content_qubes_knyt_pilot.sql` |

## Stats

 2 files changed, 267 insertions(+), 1 deletion(-)
