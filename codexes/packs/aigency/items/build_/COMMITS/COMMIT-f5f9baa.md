# Commit Brief: `f5f9baa` — fix resolveVariant: map comic_page_portrait to episode_still not episode_print

| Field | Value |
|-------|-------|
| SHA | [`f5f9baa`](https://github.com/Kn0w-1/AigentZBeta/commit/f5f9baa66022dbd9d9b47bbf2e3bd712d9d956c1) |
| Author | Claude |
| Date | 2026-05-14T22:40:04Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix resolveVariant: map comic_page_portrait to episode_still not episode_print

PDFs (comic_page_portrait items) are stored as content_type='episode_still' in
master_content_qubes. The top-knyt-investor SKU grants episode_still=true but
episode_print=false. Mapping to episode_print caused isEpisodeLocked to always
return true for PDF cards — routing all reads to the paywall instead of the viewer.
```

## Body

PDFs (comic_page_portrait items) are stored as content_type='episode_still' in
master_content_qubes. The top-knyt-investor SKU grants episode_still=true but
episode_print=false. Mapping to episode_print caused isEpisodeLocked to always
return true for PDF cards — routing all reads to the paywall instead of the viewer.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 1 insertion(+), 1 deletion(-)
