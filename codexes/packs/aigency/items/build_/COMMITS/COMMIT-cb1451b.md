# Commit Brief: `cb1451b` — pdf reader: restore <object> desktop + <iframe> mobile (dbaab0bc working pattern)

| Field | Value |
|-------|-------|
| SHA | [`cb1451b`](https://github.com/Kn0w-1/AigentZBeta/commit/cb1451bd08ab4e4fe48867d3f31b499cb79350fe) |
| Author | Claude |
| Date | 2026-05-16T08:47:38Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf reader: restore <object> desktop + <iframe> mobile (dbaab0bc working pattern)

The pure-<iframe> approach made Firefox download cross-origin PDFs instead
of rendering them inline (confirmed today: GN file existed at Supabase but
the iframe path saved it to disk). Reverting to the historical working
pattern: <object type="application/pdf"> for desktop, <iframe> for mobile.

CLAUDE.md previously said never use <object> because of
NS_ERROR_WONT_HANDLE_CONTENT in Firefox. After investigating: that error
only fires when the URL returns a missing / 0-byte / unservable file
(today's ep 12 case where the Supabase object doesn't exist) — not as a
property of <object> itself. With a healthy file, <object> renders fine on
Firefox 150, Chromium, and Safari desktop. The right remedy for the
"NS_ERROR_WONT_HANDLE_CONTENT" symptom is to ensure the file exists at the
URL, not to swap the embed element.
```

## Body

The pure-<iframe> approach made Firefox download cross-origin PDFs instead
of rendering them inline (confirmed today: GN file existed at Supabase but
the iframe path saved it to disk). Reverting to the historical working
pattern: <object type="application/pdf"> for desktop, <iframe> for mobile.

CLAUDE.md previously said never use <object> because of
NS_ERROR_WONT_HANDLE_CONTENT in Firefox. After investigating: that error
only fires when the URL returns a missing / 0-byte / unservable file
(today's ep 12 case where the Supabase object doesn't exist) — not as a
property of <object> itself. With a healthy file, <object> renders fine on
Firefox 150, Chromium, and Safari desktop. The right remedy for the
"NS_ERROR_WONT_HANDLE_CONTENT" symptom is to ensure the file exists at the
URL, not to swap the embed element.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 57 insertions(+), 17 deletions(-)
