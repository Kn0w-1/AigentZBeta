# Commit Brief: `d98eb73` — diag: log still-master CID/lite-url + accept URL in cid field

| Field | Value |
|-------|-------|
| SHA | [`d98eb73`](https://github.com/Kn0w-1/AigentZBeta/commit/d98eb73fac6b79a26b49e6997ba508528bdb3381) |
| Author | Claude |
| Date | 2026-05-15T23:55:14Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
diag: log still-master CID/lite-url + accept URL in cid field

Two changes to debug why the previous still-master fix didn't yield
stillMasterCid / stillMasterLiteUrl in the API response:

1. accept URLs in auto_drive_cid for still masters too. The legacy GN
   replacement migration (20260503010000_replace_gn_pdf.sql) stores the
   Supabase storage URL directly in master_content_qubes.auto_drive_cid.
   When that's the case for an episode_still row, surface the URL as
   stillMasterLiteUrl (matching the same hoisting pattern used for
   episode_print masters).

2. Add a server-side console.log of the still-master row's id, episode,
   and whether auto_drive_cid / pdf_lite_url are populated. This tells
   us conclusively whether the DB has URL data on the still masters or
   whether the upstream content seeding never set them.

If, after deploy, the API still doesn't return stillMasterCid /
stillMasterLiteUrl, the [CodexStatus] still master server logs will tell
us whether (a) DB rows have NULL URLs (data layer; needs content seeded)
or (b) the values are present but the response shape is dropping them.

trigger deploy to dev
```

## Body

Two changes to debug why the previous still-master fix didn't yield
stillMasterCid / stillMasterLiteUrl in the API response:

1. accept URLs in auto_drive_cid for still masters too. The legacy GN
   replacement migration (20260503010000_replace_gn_pdf.sql) stores the
   Supabase storage URL directly in master_content_qubes.auto_drive_cid.
   When that's the case for an episode_still row, surface the URL as
   stillMasterLiteUrl (matching the same hoisting pattern used for
   episode_print masters).

2. Add a server-side console.log of the still-master row's id, episode,
   and whether auto_drive_cid / pdf_lite_url are populated. This tells
   us conclusively whether the DB has URL data on the still masters or
   whether the upstream content seeding never set them.

If, after deploy, the API still doesn't return stillMasterCid /
stillMasterLiteUrl, the [CodexStatus] still master server logs will tell
us whether (a) DB rows have NULL URLs (data layer; needs content seeded)
or (b) the values are present but the response shape is dropping them.

trigger deploy to dev

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/admin/codex/status/route.ts` |

## Stats

 2 files changed, 17 insertions(+), 1 deletion(-)
