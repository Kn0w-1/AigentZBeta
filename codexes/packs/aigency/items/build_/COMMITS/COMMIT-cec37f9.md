# Commit Brief: `cec37f9` — codex GN: surface gn_still master via stillMasterCid/LiteUrl path

| Field | Value |
|-------|-------|
| SHA | [`cec37f9`](https://github.com/Kn0w-1/AigentZBeta/commit/cec37f981671d0629fc28aeee77d31c356ce5449) |
| Author | Claude |
| Date | 2026-05-16T06:50:25Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
codex GN: surface gn_still master via stillMasterCid/LiteUrl path

The master loop in /api/admin/codex/status only handled content_type
episode_still / episode_motion / episode_print. The GN row at
master_content_qubes.episode_number=-1 with content_type='gn_still'
was silently dropped, so the AGN/GN card's modalities.read never lit
up and the GN didn't open from the codex.

Two-site fix:
- status route now treats 'gn_still' the same as 'episode_still' in
  the master loop. Since the GN row's auto_drive_cid is a Supabase
  https:// URL (not an Autonomys CID), the existing isUrl branch
  hoists it into stillMasterLiteUrl — same path used by legacy
  episode_still fixtures that store the readable PDF URL there.
- KnytTab.gnEp resolution now falls through to
  stillMasterCid / stillMasterLiteUrl when no print-tier CID is set,
  so the AGN preorder card's media.pdf_cid / media.pdf_lite_url get
  populated from the gn_still master row.
```

## Body

The master loop in /api/admin/codex/status only handled content_type
episode_still / episode_motion / episode_print. The GN row at
master_content_qubes.episode_number=-1 with content_type='gn_still'
was silently dropped, so the AGN/GN card's modalities.read never lit
up and the GN didn't open from the codex.

Two-site fix:
- status route now treats 'gn_still' the same as 'episode_still' in
  the master loop. Since the GN row's auto_drive_cid is a Supabase
  https:// URL (not an Autonomys CID), the existing isUrl branch
  hoists it into stillMasterLiteUrl — same path used by legacy
  episode_still fixtures that store the readable PDF URL there.
- KnytTab.gnEp resolution now falls through to
  stillMasterCid / stillMasterLiteUrl when no print-tier CID is set,
  so the AGN preorder card's media.pdf_cid / media.pdf_lite_url get
  populated from the gn_still master row.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/admin/codex/status/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 3 files changed, 11 insertions(+), 6 deletions(-)
