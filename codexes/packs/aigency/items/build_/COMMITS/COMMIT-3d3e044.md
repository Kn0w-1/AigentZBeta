# Commit Brief: `3d3e044` — fix RemixDialog 'personaId required' 400 — resolve via spine

| Field | Value |
|-------|-------|
| SHA | [`3d3e044`](https://github.com/Kn0w-1/AigentZBeta/commit/3d3e044e2f4f0c13131eedaa0d0f58e9218d3844) |
| Author | Claude |
| Date | 2026-05-22T02:07:23Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix RemixDialog 'personaId required' 400 — resolve via spine

Two-part fix for the 400 that now surfaces after disabling the client-side
sign-in gate:

Server (/api/community-content/generate):
  Fall back to getActivePersona(request).personaId when body.personaId is
  absent — identity flows from Authorization: Bearer per CLAUDE.md spine
  rule. Returns 401 'sign-in required' (was 400 'personaId required') when
  neither resolves.

Client (RemixDialog):
  Switch the four community-content fetch calls (quota, generate, discard,
  publish) to personaFetch so the Supabase access token rides as
  Authorization: Bearer and the spine can resolve the active persona
  server-side even when the body lacks personaId.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym
```

## Body

Two-part fix for the 400 that now surfaces after disabling the client-side
sign-in gate:

Server (/api/community-content/generate):
  Fall back to getActivePersona(request).personaId when body.personaId is
  absent — identity flows from Authorization: Bearer per CLAUDE.md spine
  rule. Returns 401 'sign-in required' (was 400 'personaId required') when
  neither resolves.

Client (RemixDialog):
  Switch the four community-content fetch calls (quota, generate, discard,
  publish) to personaFetch so the Supabase access token rides as
  Authorization: Bearer and the spine can resolve the active persona
  server-side even when the body lacks personaId.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/generate/route.ts` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 2 files changed, 20 insertions(+), 7 deletions(-)
