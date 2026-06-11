# Commit Brief: `27df22d` — codex convention: fix ep-0 falsy-skip and store thumbnail -1 offset

| Field | Value |
|-------|-------|
| SHA | [`27df22d`](https://github.com/Kn0w-1/AigentZBeta/commit/27df22df0bf28382ba8564a575d4368edd469e5e) |
| Author | Claude |
| Date | 2026-05-16T06:35:12Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
codex convention: fix ep-0 falsy-skip and store thumbnail -1 offset

Two regressions exposed by the metaKnyts data realignment:

1) status route was using `if (ep.episode_number && …)` to gate the
   codex_episodes → metadataMap merge. After the SQL shift, "Gen Zero
   Divided by One" sits at codex_episodes.episode_number = 0, which is
   falsy in JS, so the title row was silently skipped. The codex Ep #0
   card was rendering as "Episode #0 / Episode #0" (fallback) instead of
   "Episode #0 / Gen Zero Divided by One". Changed to an explicit
   != null check that keeps 0 and rejects null/undefined.

2) /api/knyt/thumbnails was returning covers with
   `episodeNumber: db.episode_number - 1` from the old convention (DB 0
   = GN, DB 1 = display #0). After the codex_media_assets shift the DB
   episode_number IS the display number, so the subtraction was
   double-shifting and the store's getCoverThumb(N) was reaching one
   slot too high — AGN/GN row pulled the archer (ep 0's real cover),
   every following episode pulled the one above it, and ep #12 had no
   cover because nothing exists at DB ep 13 anymore. Covers now pass
   through unchanged. Characters still subtract 1 (codex_media_assets
   characters remain 1-indexed by separate convention).
```

## Body

Two regressions exposed by the metaKnyts data realignment:

1) status route was using `if (ep.episode_number && …)` to gate the
   codex_episodes → metadataMap merge. After the SQL shift, "Gen Zero
   Divided by One" sits at codex_episodes.episode_number = 0, which is
   falsy in JS, so the title row was silently skipped. The codex Ep #0
   card was rendering as "Episode #0 / Episode #0" (fallback) instead of
   "Episode #0 / Gen Zero Divided by One". Changed to an explicit
   != null check that keeps 0 and rejects null/undefined.

2) /api/knyt/thumbnails was returning covers with
   `episodeNumber: db.episode_number - 1` from the old convention (DB 0
   = GN, DB 1 = display #0). After the codex_media_assets shift the DB
   episode_number IS the display number, so the subtraction was
   double-shifting and the store's getCoverThumb(N) was reaching one
   slot too high — AGN/GN row pulled the archer (ep 0's real cover),
   every following episode pulled the one above it, and ep #12 had no
   cover because nothing exists at DB ep 13 anymore. Covers now pass
   through unchanged. Characters still subtract 1 (codex_media_assets
   characters remain 1-indexed by separate convention).

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/admin/codex/status/route.ts` |
| Modified | `app/api/knyt/thumbnails/route.ts` |

## Stats

 3 files changed, 12 insertions(+), 6 deletions(-)
