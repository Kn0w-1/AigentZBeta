# Commit Brief: `b3f62de` — community-content: publish + discard accept implicit identity via spine

| Field | Value |
|-------|-------|
| SHA | [`b3f62de`](https://github.com/Kn0w-1/AigentZBeta/commit/b3f62de640a179d749747531767acac1c4ecb990) |
| Author | Claude |
| Date | 2026-05-23T00:57:59Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
community-content: publish + discard accept implicit identity via spine

Publishing a remixed Experience Qube from myCanvas was returning
'personaId required' because /publish hard-required body.personaId
and the client (MyCanvasTab.handlePublishToCommunity) sends an
empty body, relying on personaFetch's Bearer + x-persona-id header
to identify the persona — same pattern that /generate already
supports.

Add the same spine fallback to /publish and /discard:
  • body.personaId still preferred when present (back-compat)
  • when absent, getActivePersona(req) resolves via the JWT and the
    x-persona-id header personaFetch attaches from localStorage
  • when neither yields a persona, return 401 'sign-in required'
    instead of the misleading 400 'personaId required'

Creator-only / status guards stay exactly as they were — once
personaId is resolved, the existing checks (creator_persona_id !==
personaId → 403, status guards, discard window) run unchanged.
```

## Body

Publishing a remixed Experience Qube from myCanvas was returning
'personaId required' because /publish hard-required body.personaId
and the client (MyCanvasTab.handlePublishToCommunity) sends an
empty body, relying on personaFetch's Bearer + x-persona-id header
to identify the persona — same pattern that /generate already
supports.

Add the same spine fallback to /publish and /discard:
  • body.personaId still preferred when present (back-compat)
  • when absent, getActivePersona(req) resolves via the JWT and the
    x-persona-id header personaFetch attaches from localStorage
  • when neither yields a persona, return 401 'sign-in required'
    instead of the misleading 400 'personaId required'

Creator-only / status guards stay exactly as they were — once
personaId is resolved, the existing checks (creator_persona_id !==
personaId → 403, status guards, discard window) run unchanged.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/[id]/discard/route.ts` |
| Modified | `app/api/community-content/[id]/publish/route.ts` |

## Stats

 2 files changed, 32 insertions(+), 4 deletions(-)
