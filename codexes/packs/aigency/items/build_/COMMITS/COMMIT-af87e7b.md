# Commit Brief: `af87e7b` — pdf reader: stop covering rendered PDFs with a spurious "timed out" overlay

| Field | Value |
|-------|-------|
| SHA | [`af87e7b`](https://github.com/Kn0w-1/AigentZBeta/commit/af87e7b332f93c3db643c8a744f98ad3b64af58e) |
| Author | Claude |
| Date | 2026-05-16T21:10:23Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf reader: stop covering rendered PDFs with a spurious "timed out" overlay

Root cause of the GN "Preview timed out" message: <object>.onLoad fires
inconsistently or never for cross-origin PDFs in Brave/Chromium. The
wall-clock timer that was supposed to dismiss the spinner was instead
painting a "Preview timed out" error overlay over PDFs that had already
rendered underneath. Larger files made it worse — more time spent loading
meant more chance the timer fired before users realized the PDF was
visible.

New strategy: short spinner-only delay (5s first load, 1.5s repeat),
never show a "timed out" overlay. The <object> element either paints
the PDF (success) or shows the browser's native error UI (genuine
failure). The wall-clock approach was wrong because we have no reliable
signal that the load failed vs. is slow vs. succeeded but didn't fire
onLoad.
```

## Body

Root cause of the GN "Preview timed out" message: <object>.onLoad fires
inconsistently or never for cross-origin PDFs in Brave/Chromium. The
wall-clock timer that was supposed to dismiss the spinner was instead
painting a "Preview timed out" error overlay over PDFs that had already
rendered underneath. Larger files made it worse — more time spent loading
meant more chance the timer fired before users realized the PDF was
visible.

New strategy: short spinner-only delay (5s first load, 1.5s repeat),
never show a "timed out" overlay. The <object> element either paints
the PDF (success) or shows the browser's native error UI (genuine
failure). The wall-clock approach was wrong because we have no reliable
signal that the load failed vs. is slow vs. succeeded but didn't fire
onLoad.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 23 insertions(+), 50 deletions(-)
