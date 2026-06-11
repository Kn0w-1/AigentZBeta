# Commit Brief: `a1b932b` — restore: KnytTab + PDFLiteReaderModal to build 1ce1da07 state

| Field | Value |
|-------|-------|
| SHA | [`a1b932b`](https://github.com/Kn0w-1/AigentZBeta/commit/a1b932b3553a42f36a6c903c6e3f640228bfa431) |
| Author | Claude |
| Date | 2026-05-16T03:36:53Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
restore: KnytTab + PDFLiteReaderModal to build 1ce1da07 state

Reverts all gnEp offset changes back to the pre-offset build the operator
requested. KnytTab: episodeNumber === 0 captures GN, episodes -1..-4 skipped.
PDFLiteReaderModal: 24s timeout restored.
```

## Body

Reverts all gnEp offset changes back to the pre-offset build the operator
requested. KnytTab: episodeNumber === 0 captures GN, episodes -1..-4 skipped.
PDFLiteReaderModal: 24s timeout restored.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 6 insertions(+), 9 deletions(-)
