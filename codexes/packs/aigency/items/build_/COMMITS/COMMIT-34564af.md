# Commit Brief: `34564af` — pdf reader: bump timeouts further + "this may take a few minutes" hint

| Field | Value |
|-------|-------|
| SHA | [`34564af`](https://github.com/Kn0w-1/AigentZBeta/commit/34564afa99d0160c70ed3511895defc2e9da3ff6) |
| Author | Claude |
| Date | 2026-05-16T21:05:53Z |
| Branch | dev (direct push) |
| Type | `chore` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pdf reader: bump timeouts further + "this may take a few minutes" hint

First-load timeout 120s -> 600s (10 minutes), repeat-load 600s -> 900s
(15 minutes). 430MB GN over a typical home link can exceed 2 minutes —
the old 120s bound was still tripping for users on slower connections
even when the download was making progress.

After 30 seconds of loading, the spinner caption swaps from
"Loading PDF…" to "Loading large file — this may take a few minutes…"
so users on a slow first-load know the modal hasn't frozen.
```

## Body

First-load timeout 120s -> 600s (10 minutes), repeat-load 600s -> 900s
(15 minutes). 430MB GN over a typical home link can exceed 2 minutes —
the old 120s bound was still tripping for users on slower connections
even when the download was making progress.

After 30 seconds of loading, the spinner caption swaps from
"Loading PDF…" to "Loading large file — this may take a few minutes…"
so users on a slow first-load know the modal hasn't frozen.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/content/PDFLiteReaderModal.tsx` |

## Stats

 2 files changed, 14 insertions(+), 5 deletions(-)
