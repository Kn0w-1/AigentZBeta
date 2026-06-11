# Commit Brief: `eced916` — fix: KnytTab off-by-one — capture GN at episode_number=-1, not 0

| Field | Value |
|-------|-------|
| SHA | [`eced916`](https://github.com/Kn0w-1/AigentZBeta/commit/eced916dd6e19aaa298cca1b61fb98a6d3f05049) |
| Author | Claude |
| Date | 2026-05-16T02:44:52Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: KnytTab off-by-one — capture GN at episode_number=-1, not 0

The canonical convention (per /api/admin/codex/canonical) is:
  episode_number = -1 → GN (content_type='gn_still')
  episode_number =  0..12 → the 13 regular episodes

The legacy capture line treated DB ep 0 as the GN, which absorbed the
first real episode into the AGN slot and pushed every other episode's
CID one position forward in the grid:
  display #0 card → opened display #1's CID
  display #1 card → opened display #2's CID
  ...
  display #11 card → opened display #12's CID (the long one that
  was timing out)

Switching to episode_number === -1 leaves DB eps 0..12 as their own
cards, each with its own master_content_qubes.auto_drive_cid. GN
handling (still needs API surfacing of content_type='gn_still') is
out of scope of this commit — gnEp will be null until that's wired,
so the AGN card has no readable PDF for now, but every regular
episode card opens the correct content.

Also bump PDFLiteReaderModal load timeout 24s → 48s. The previous
24s timeout was set for the GN; the longest episode PDF (display
#12) is even larger and was tripping the safety-net timeout before
Firefox's pdf.js could call onLoad.
```

## Body

The canonical convention (per /api/admin/codex/canonical) is:
  episode_number = -1 → GN (content_type='gn_still')
  episode_number =  0..12 → the 13 regular episodes

The legacy capture line treated DB ep 0 as the GN, which absorbed the
first real episode into the AGN slot and pushed every other episode's
CID one position forward in the grid:
  display #0 card → opened display #1's CID
  display #1 card → opened display #2's CID
  ...
  display #11 card → opened display #12's CID (the long one that
  was timing out)

Switching to episode_number === -1 leaves DB eps 0..12 as their own
cards, each with its own master_content_qubes.auto_drive_cid. GN
handling (still needs API surfacing of content_type='gn_still') is
out of scope of this commit — gnEp will be null until that's wired,
so the AGN card has no readable PDF for now, but every regular
episode card opens the correct content.

Also bump PDFLiteReaderModal load timeout 24s → 48s. The previous
24s timeout was set for the GN; the longest episode PDF (display
#12) is even larger and was tripping the safety-net timeout before
Firefox's pdf.js could call onLoad.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 15 insertions(+), 6 deletions(-)
