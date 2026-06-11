# Commit Brief: `1954bbc` — fix: open PDF / motion / Coming-Soon fallback on owned-card click

| Field | Value |
|-------|-------|
| SHA | [`1954bbc`](https://github.com/Kn0w-1/AigentZBeta/commit/1954bbc704fad2f133459fd99293e5e57155bf08) |
| Author | Claude |
| Date | 2026-05-15T23:11:21Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: open PDF / motion / Coming-Soon fallback on owned-card click

User's log shows clicks on owned `comic_cover_portrait` items (mk_ep04,
mk_ep09, mk_ep02) suppress the buy modal but then no viewer opens. Root
cause: cover-only items (rendered when API returns the episode with
hasReadable=false && hasWatchable=false && hasCover=true) have no
media.pdf_*; the original fallback only checked episodesCatalog.find(...)
and silently returned when the catalog entry ALSO lacked print URLs.

Now the buy-suppression path in handleSmartAction:
- Logs the resolved episodeNumber, catalog match, available URLs
- Tries item.media.pdf_lite_url / pdf_cid as a secondary fallback
- Tries openEpisodeVideo with motionMasterCid as a tertiary fallback
- Falls back to a 'Owned · Content not yet published' toast (preserves
  the user's expectation that owned-but-unpublished content surfaces as
  Coming Soon rather than silently doing nothing)

trigger deploy to dev
```

## Body

User's log shows clicks on owned `comic_cover_portrait` items (mk_ep04,
mk_ep09, mk_ep02) suppress the buy modal but then no viewer opens. Root
cause: cover-only items (rendered when API returns the episode with
hasReadable=false && hasWatchable=false && hasCover=true) have no
media.pdf_*; the original fallback only checked episodesCatalog.find(...)
and silently returned when the catalog entry ALSO lacked print URLs.

Now the buy-suppression path in handleSmartAction:
- Logs the resolved episodeNumber, catalog match, available URLs
- Tries item.media.pdf_lite_url / pdf_cid as a secondary fallback
- Tries openEpisodeVideo with motionMasterCid as a tertiary fallback
- Falls back to a 'Owned · Content not yet published' toast (preserves
  the user's expectation that owned-but-unpublished content surfaces as
  Coming Soon rather than silently doing nothing)

trigger deploy to dev

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 44 insertions(+), 14 deletions(-)
