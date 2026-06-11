# Commit Brief: `30cce06` — pdf viewer routing: desktop -> PDFLiteReaderModal, mobile -> PDFPageViewer

| Field | Value |
|-------|-------|
| SHA | [`30cce06`](https://github.com/Kn0w-1/AigentZBeta/commit/30cce066e5fff2304e7c7bc0570ec55b11f3a9ab) |
| Author | Claude |
| Date | 2026-05-16T17:45:35Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf viewer routing: desktop -> PDFLiteReaderModal, mobile -> PDFPageViewer

Operator directive (tried-and-tested pattern): on desktop/tablet we always
use PDFLiteReaderModal (<object type="application/pdf"> on the Supabase
pdf_lite_url), and on mobile we use PDFPageViewer (page-by-page via the
/api/content/pdf-page proxy). Previously the split was based on which
field was populated on the episode row rather than viewport, which sent
desktop users into the page-by-page viewer whenever pdf_lite_url was
null — and the page-by-page viewer is itself unreliable today.

Keeps the PDFPageViewer fallback wired for the desktop "no pdf_lite_url"
edge case (currently ep 12, whose Supabase upload is missing). That fallback
is temporary — once we either re-upload the file or add a server proxy that
exposes the Autonomys CID inline, the desktop branch goes directly through
PDFLiteReaderModal.
```

## Body

Operator directive (tried-and-tested pattern): on desktop/tablet we always
use PDFLiteReaderModal (<object type="application/pdf"> on the Supabase
pdf_lite_url), and on mobile we use PDFPageViewer (page-by-page via the
/api/content/pdf-page proxy). Previously the split was based on which
field was populated on the episode row rather than viewport, which sent
desktop users into the page-by-page viewer whenever pdf_lite_url was
null — and the page-by-page viewer is itself unreliable today.

Keeps the PDFPageViewer fallback wired for the desktop "no pdf_lite_url"
edge case (currently ep 12, whose Supabase upload is missing). That fallback
is temporary — once we either re-upload the file or add a server proxy that
exposes the Autonomys CID inline, the desktop branch goes directly through
PDFLiteReaderModal.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 28 insertions(+), 9 deletions(-)
