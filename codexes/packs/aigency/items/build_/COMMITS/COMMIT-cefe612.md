# Commit Brief: `cefe612` — fix: GN PDF open — surface gn_still URL + capture gnEp at -1

| Field | Value |
|-------|-------|
| SHA | [`cefe612`](https://github.com/Kn0w-1/AigentZBeta/commit/cefe6129e03b6c987a25cc1a6101b6bce576d31f) |
| Author | Claude |
| Date | 2026-05-16T01:37:27Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: GN PDF open — surface gn_still URL + capture gnEp at -1

The GN (master_content_qubes row mk_ep00_print_common, content_type
'gn_still', episode_number=-1) has its Supabase Storage URL stored in
auto_drive_cid (no Autonomys CID yet, since the GN is not minted).

Two changes restore the AGN card → PDFLiteReaderModal path:

1. /api/admin/codex/status: the master loop now treats content_type
   'gn_still' the same as 'episode_still' — extracting auto_drive_cid /
   pdf_lite_url into stillMasterCid / stillMasterLiteUrl. Since the GN's
   auto_drive_cid is a URL, it surfaces as stillMasterLiteUrl.

2. KnytTab.transformEpisodesToContentItems: gnEp capture switched from
   'episodeNumber === 0' (legacy convention) to 'episodeNumber === -1'
   (canonical convention per /api/admin/codex/canonical). The
   gnPrintCid / gnPrintLiteUrl resolution chain now also falls back to
   gnEp.stillMasterCid / gnEp.stillMasterLiteUrl so the AGN card has a
   working read source even when only the gn_still row is populated.
```

## Body

The GN (master_content_qubes row mk_ep00_print_common, content_type
'gn_still', episode_number=-1) has its Supabase Storage URL stored in
auto_drive_cid (no Autonomys CID yet, since the GN is not minted).

Two changes restore the AGN card → PDFLiteReaderModal path:

1. /api/admin/codex/status: the master loop now treats content_type
   'gn_still' the same as 'episode_still' — extracting auto_drive_cid /
   pdf_lite_url into stillMasterCid / stillMasterLiteUrl. Since the GN's
   auto_drive_cid is a URL, it surfaces as stillMasterLiteUrl.

2. KnytTab.transformEpisodesToContentItems: gnEp capture switched from
   'episodeNumber === 0' (legacy convention) to 'episodeNumber === -1'
   (canonical convention per /api/admin/codex/canonical). The
   gnPrintCid / gnPrintLiteUrl resolution chain now also falls back to
   gnEp.stillMasterCid / gnEp.stillMasterLiteUrl so the AGN card has a
   working read source even when only the gn_still row is populated.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/admin/codex/status/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 17 insertions(+), 8 deletions(-)
