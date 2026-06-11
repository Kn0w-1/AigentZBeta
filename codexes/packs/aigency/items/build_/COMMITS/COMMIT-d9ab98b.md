# Commit Brief: `d9ab98b` — Phase B: ContentQube registry as canonical inventory + ownership SOT

| Field | Value |
|-------|-------|
| SHA | [`d9ab98b`](https://github.com/Kn0w-1/AigentZBeta/commit/d9ab98b57169973efdc25686522434db86dbe816) |
| Author | Claude |
| Date | 2026-05-14T18:19:12Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Phase B: ContentQube registry as canonical inventory + ownership SOT

Operator-locked decision: every shelf / codex tab / click handler should
resolve 'does this persona own this content?' and 'what is the canonical
inventory of this series?' through the SAME pipeline — the ContentQube
registry. Phase A made the legacy /api/codex/owned variant-aware; this
phase canonicalizes the four parallel ownership paths onto one registry-
backed surface.

New surfaces:
- GET /api/registry/content-qube/series-rights — returns the union of:
  1. Real content_qubes rows for the series (persona_owns via evaluateAccess)
  2. Synthesized rights-grant placeholders (is_placeholder=true,
     lifecycle_state='draft') for slots the persona has SKU rights to but
     where no master_content_qubes row exists yet (e.g. ep1/ep3 motion).
  These placeholders are an in-memory composition off
  getOwnedAssetIds().expectedSlots — NO master_content_qubes pollution.
- useContentQubeSeriesRights hook (mirrors useContentQubeSeries, calls the
  new endpoint, forwards personaId explicitly per the iframe-cookie rule).
- ContentQubeDisplayManifest.is_placeholder?: boolean — additive,
  back-compat for existing consumers that ignore it.

Migrations:
- KnytShelfTab: switched off useOwnedEntitlements → useContentQubeSeriesRights.
  Renders one tile per (episode, motion-flag) so the operator-expected
  40-item count (13 stills + 13 motions + 13 cards + 1 GN) holds for the
  Top KNYT Shelf persona, with ep1/ep3 motion surfacing as 'Owned · Coming
  Soon' placeholders. Stable sort: episodes → cards → GN; within episodes
  stills before motions, ordered by episode_number.
- KnytTab.isEpisodeLocked + openEpisodeVideo: ContentQube registry is now
  the PRIMARY ownership check; legacy ownedIssues (variant-aware after
  Phase A) is consulted only when the registry has no entry for the
  requested (episode, variant) pair. Fixes the 'Owned badge → payment
  modal' symptom on motion variants even when the master row exists but
  the SKU's grants_episodes_motion flag is true. Episode 12 specifically:
  if rights exist via SKU expansion but master row absent, the rights
  placeholder surfaces and unlocks.
- ScrollsTab + CharactersTab: swapped useContentQubeSeries →
  useContentQubeSeriesRights so they pick up the placeholders too.
  Episode 12 will badge as Owned via the SKU-rights placeholder when
  the persona has rights but no master row exists.

Deprecation headers:
- /api/codex/owned + useOwnedEntitlements get @deprecated comments
  pointing readers to the registry hook. Not removed — store admin tabs
  and the bundle wizard still consume them. Removal is a follow-up.

Rarity-level per-column ownership in ScrollsTab remains a Phase C
backlog item (manifest.persona_owns is a single boolean across all
rarities today).
```

## Body

Operator-locked decision: every shelf / codex tab / click handler should
resolve 'does this persona own this content?' and 'what is the canonical
inventory of this series?' through the SAME pipeline — the ContentQube
registry. Phase A made the legacy /api/codex/owned variant-aware; this
phase canonicalizes the four parallel ownership paths onto one registry-
backed surface.

New surfaces:
- GET /api/registry/content-qube/series-rights — returns the union of:
  1. Real content_qubes rows for the series (persona_owns via evaluateAccess)
  2. Synthesized rights-grant placeholders (is_placeholder=true,
     lifecycle_state='draft') for slots the persona has SKU rights to but
     where no master_content_qubes row exists yet (e.g. ep1/ep3 motion).
  These placeholders are an in-memory composition off
  getOwnedAssetIds().expectedSlots — NO master_content_qubes pollution.
- useContentQubeSeriesRights hook (mirrors useContentQubeSeries, calls the
  new endpoint, forwards personaId explicitly per the iframe-cookie rule).
- ContentQubeDisplayManifest.is_placeholder?: boolean — additive,
  back-compat for existing consumers that ignore it.

Migrations:
- KnytShelfTab: switched off useOwnedEntitlements → useContentQubeSeriesRights.
  Renders one tile per (episode, motion-flag) so the operator-expected
  40-item count (13 stills + 13 motions + 13 cards + 1 GN) holds for the
  Top KNYT Shelf persona, with ep1/ep3 motion surfacing as 'Owned · Coming
  Soon' placeholders. Stable sort: episodes → cards → GN; within episodes
  stills before motions, ordered by episode_number.
- KnytTab.isEpisodeLocked + openEpisodeVideo: ContentQube registry is now
  the PRIMARY ownership check; legacy ownedIssues (variant-aware after
  Phase A) is consulted only when the registry has no entry for the
  requested (episode, variant) pair. Fixes the 'Owned badge → payment
  modal' symptom on motion variants even when the master row exists but
  the SKU's grants_episodes_motion flag is true. Episode 12 specifically:
  if rights exist via SKU expansion but master row absent, the rights
  placeholder surfaces and unlocks.
- ScrollsTab + CharactersTab: swapped useContentQubeSeries →
  useContentQubeSeriesRights so they pick up the placeholders too.
  Episode 12 will badge as Owned via the SKU-rights placeholder when
  the persona has rights but no master row exists.

Deprecation headers:
- /api/codex/owned + useOwnedEntitlements get @deprecated comments
  pointing readers to the registry hook. Not removed — store admin tabs
  and the bundle wizard still consume them. Removal is a follow-up.

Rarity-level per-column ownership in ScrollsTab remains a Phase C
backlog item (manifest.persona_owns is a single boolean across all
rarities today).

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/codex/owned/route.ts` |
| Added | `app/api/registry/content-qube/series-rights/route.ts` |
| Modified | `app/hooks/useOwnedEntitlements.ts` |
| Modified | `app/triad/components/codex/tabs/CharactersTab.tsx` |
| Modified | `app/triad/components/codex/tabs/KnytShelfTab.tsx` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |
| Modified | `app/triad/components/codex/tabs/ScrollsTab.tsx` |
| Added | `app/triad/components/codex/tabs/useContentQubeSeriesRights.ts` |
| Modified | `types/contentQube.ts` |

## Stats

 9 files changed, 602 insertions(+), 116 deletions(-)
