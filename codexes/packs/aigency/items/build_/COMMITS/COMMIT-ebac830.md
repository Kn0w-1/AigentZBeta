# Commit Brief: `ebac830` — pdf reader: 120s first-load timeout, 10min repeat-load, remember-loaded cache

| Field | Value |
|-------|-------|
| SHA | [`ebac830`](https://github.com/Kn0w-1/AigentZBeta/commit/ebac830bcf3571b56a1e4ccae571436582428a46) |
| Author | Claude |
| Date | 2026-05-16T20:39:57Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf reader: 120s first-load timeout, 10min repeat-load, remember-loaded cache

Two fixes for large-file UX:

1. Wall-clock timeout bumped from 24s to 120s for first loads. The 430MB
   GN takes 30-60+ seconds on typical links and was hitting the old wall
   even when the PDF was visibly rendering — <object>'s onLoad event fires
   inconsistently for cross-origin PDFs, so the timer is the
   spinner-dismissal fallback. 24s wasn't enough headroom.

2. Per-URL 'previously loaded' cache in localStorage. When onLoad fires
   for a URL, we record the timestamp. Next time the same URL opens, we
   extend the timeout to 10 minutes — the browser HTTP cache will serve
   the bytes near-instantly, so we don't want to flash a spurious
   timeout message over an already-rendered PDF. Entry TTL is 7 days,
   ~one entry per metaKnyts file so the storage footprint is tiny.

The actual byte-caching is the browser's job (HTTP cache, with
cache-control: max-age=3600 set at upload). This change just stops the
modal from giving up early when the cache is doing its job.
```

## Body

Two fixes for large-file UX:

1. Wall-clock timeout bumped from 24s to 120s for first loads. The 430MB
   GN takes 30-60+ seconds on typical links and was hitting the old wall
   even when the PDF was visibly rendering — <object>'s onLoad event fires
   inconsistently for cross-origin PDFs, so the timer is the
   spinner-dismissal fallback. 24s wasn't enough headroom.

2. Per-URL 'previously loaded' cache in localStorage. When onLoad fires
   for a URL, we record the timestamp. Next time the same URL opens, we
   extend the timeout to 10 minutes — the browser HTTP cache will serve
   the bytes near-instantly, so we don't want to flash a spurious
   timeout message over an already-rendered PDF. Entry TTL is 7 days,
   ~one entry per metaKnyts file so the storage footprint is tiny.

The actual byte-caching is the browser's job (HTTP cache, with
cache-control: max-age=3600 set at upload). This change just stops the
modal from giving up early when the cache is doing its job.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 57 insertions(+), 3 deletions(-)
