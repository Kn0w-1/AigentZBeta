# Commit Brief: `34cc993` — pdf reader: use <iframe> on every viewport, drop the <object> desktop branch

| Field | Value |
|-------|-------|
| SHA | [`34cc993`](https://github.com/Kn0w-1/AigentZBeta/commit/34cc9939a55022967447da40f59cac311debeeee) |
| Author | Claude |
| Date | 2026-05-16T07:45:10Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf reader: use <iframe> on every viewport, drop the <object> desktop branch

Per CLAUDE.md and confirmed in production: <object type="application/pdf">
makes Firefox throw NS_ERROR_WONT_HANDLE_CONTENT and refuse to render
cross-origin PDFs inline. The response transfers as 0 bytes and the embed
never paints, exactly what GN and the previous attempts at Supabase-hosted
masters were hitting on dev-beta.aigentz.me.

The earlier desktop <object> branch was added to work around a separate
Firefox "downloads instead of renders" regression that has since been
resolved upstream — keeping the <object> path past that point traded one
Firefox bug for a worse one. <iframe> renders inline on Firefox, Chromium,
Safari desktop, iOS Safari, and Android Chrome, so the same element can
serve every viewport.
```

## Body

Per CLAUDE.md and confirmed in production: <object type="application/pdf">
makes Firefox throw NS_ERROR_WONT_HANDLE_CONTENT and refuse to render
cross-origin PDFs inline. The response transfers as 0 bytes and the embed
never paints, exactly what GN and the previous attempts at Supabase-hosted
masters were hitting on dev-beta.aigentz.me.

The earlier desktop <object> branch was added to work around a separate
Firefox "downloads instead of renders" regression that has since been
resolved upstream — keeping the <object> path past that point traded one
Firefox bug for a worse one. <iframe> renders inline on Firefox, Chromium,
Safari desktop, iOS Safari, and Android Chrome, so the same element can
serve every viewport.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 17 insertions(+), 54 deletions(-)
