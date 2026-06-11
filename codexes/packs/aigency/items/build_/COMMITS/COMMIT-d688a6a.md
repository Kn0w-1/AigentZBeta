# Commit Brief: `d688a6a` — fix: expose still-master CID + lite URL for readable PDF episodes

| Field | Value |
|-------|-------|
| SHA | [`d688a6a`](https://github.com/Kn0w-1/AigentZBeta/commit/d688a6ae15472efbff01ca57f8ca0d18751244c9) |
| Author | Claude |
| Date | 2026-05-15T23:39:04Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: expose still-master CID + lite URL for readable PDF episodes

API response for owned episodes showed:
  hasStillMaster: true
  stillMasterId: "mk_ep09_print_rare"
  hasPrintRare/Epic/Legendary/Common: false
  printRare*Cid/LiteUrl: NOT in response

The legacy fixture stores the readable episode PDF under
content_type='episode_still', not 'episode_print'. The status route was
only extracting auto_drive_cid + pdf_lite_url from 'episode_print'
masters, dropping the still master URL on the floor — so every regular
metaKnyts episode rendered as a cover-only card (comic_cover_portrait)
with no media.pdf_*, and clicks couldn't open the PDF.

Fix:
- app/api/admin/codex/status: also extract auto_drive_cid + pdf_lite_url
  from episode_still masters and surface them as stillMasterCid /
  stillMasterLiteUrl.
- app/triad/.../KnytTab transform: when no printRare*/Epic*/Legendary*/
  Common* URL is present, fall back to stillMasterCid / stillMasterLiteUrl.
  This makes hasReadable true → comic_page_portrait → modalities.read
  populated → click opens reader.
- owned-buy fallback in handleSmartAction: same still-master fallback so
  even cover items that ride the cover-only render path can still find
  the PDF when the persona owns the episode.

Note (operator concern, NOT addressed here): the PDF viewer is opened
client-side from a URL surfaced by the status API. Spine-canonical
content delivery should resolve through evaluateAccess → state-C
streaming. To be revisited after content is verified accessible.

trigger deploy to dev
```

## Body

API response for owned episodes showed:
  hasStillMaster: true
  stillMasterId: "mk_ep09_print_rare"
  hasPrintRare/Epic/Legendary/Common: false
  printRare*Cid/LiteUrl: NOT in response

The legacy fixture stores the readable episode PDF under
content_type='episode_still', not 'episode_print'. The status route was
only extracting auto_drive_cid + pdf_lite_url from 'episode_print'
masters, dropping the still master URL on the floor — so every regular
metaKnyts episode rendered as a cover-only card (comic_cover_portrait)
with no media.pdf_*, and clicks couldn't open the PDF.

Fix:
- app/api/admin/codex/status: also extract auto_drive_cid + pdf_lite_url
  from episode_still masters and surface them as stillMasterCid /
  stillMasterLiteUrl.
- app/triad/.../KnytTab transform: when no printRare*/Epic*/Legendary*/
  Common* URL is present, fall back to stillMasterCid / stillMasterLiteUrl.
  This makes hasReadable true → comic_page_portrait → modalities.read
  populated → click opens reader.
- owned-buy fallback in handleSmartAction: same still-master fallback so
  even cover items that ride the cover-only render path can still find
  the PDF when the persona owns the episode.

Note (operator concern, NOT addressed here): the PDF viewer is opened
client-side from a URL surfaced by the status API. Spine-canonical
content delivery should resolve through evaluateAccess → state-C
streaming. To be revisited after content is verified accessible.

trigger deploy to dev

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/admin/codex/status/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 3 files changed, 27 insertions(+), 5 deletions(-)
