# Commit Brief: `810dee8` — Phase 8: wire KNYT tab components to ContentQube registry

| Field | Value |
|-------|-------|
| SHA | [`810dee8`](https://github.com/Kn0w-1/AigentZBeta/commit/810dee8e2555c9e183f467c4ce02bf68d6caba70) |
| Author | Claude |
| Date | 2026-05-13T21:03:24Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Phase 8: wire KNYT tab components to ContentQube registry

- app/api/registry/content-qube/series/route.ts: new batch resolver endpoint
  (GET ?series=&contentKind=&lifecycleState=); calls resolveContentQubesBySeries
  so persona_owns is always evaluated server-side via evaluateAccess
- app/triad/components/codex/tabs/useContentQubeSeries.ts: React hook with
  3-min module-level cache; fetches manifests + editionSummary per series/kind
- ScrollsTab: consumes useContentQubeSeries('metaKnyts', {contentKind:'episode'});
  lock→unlock icon driven by persona_owns from registry (both by-episode and
  grid views); personaId prop now forwarded (was prefixed _personaId)
- CharactersTab: replaces MOCK_CHARACTERS with real ContentQube character
  manifests; shows title, episode index, owned/gated state, price_qc display
- add Base TokenQube activation backlog (2026-05-13_base-tokenqube-activation-backlog.md)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

- app/api/registry/content-qube/series/route.ts: new batch resolver endpoint
  (GET ?series=&contentKind=&lifecycleState=); calls resolveContentQubesBySeries
  so persona_owns is always evaluated server-side via evaluateAccess
- app/triad/components/codex/tabs/useContentQubeSeries.ts: React hook with
  3-min module-level cache; fetches manifests + editionSummary per series/kind
- ScrollsTab: consumes useContentQubeSeries('metaKnyts', {contentKind:'episode'});
  lock→unlock icon driven by persona_owns from registry (both by-episode and
  grid views); personaId prop now forwarded (was prefixed _personaId)
- CharactersTab: replaces MOCK_CHARACTERS with real ContentQube character
  manifests; shows title, episode index, owned/gated state, price_qc display
- add Base TokenQube activation backlog (2026-05-13_base-tokenqube-activation-backlog.md)

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/registry/content-qube/series/route.ts` |
| Modified | `app/triad/components/codex/tabs/CharactersTab.tsx` |
| Modified | `app/triad/components/codex/tabs/ScrollsTab.tsx` |
| Added | `app/triad/components/codex/tabs/useContentQubeSeries.ts` |
| Modified | `codexes/packs/agentiq/collections.json` |
| Added | `codexes/packs/agentiq/updates/2026-05-13_base-tokenqube-activation-backlog.md` |

## Stats

 6 files changed, 390 insertions(+), 39 deletions(-)
