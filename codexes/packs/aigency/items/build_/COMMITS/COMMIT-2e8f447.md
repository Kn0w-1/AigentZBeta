# Commit Brief: `2e8f447` — Fix iframe-context persona resolution in registry routes

| Field | Value |
|-------|-------|
| SHA | [`2e8f447`](https://github.com/Kn0w-1/AigentZBeta/commit/2e8f447234c71d20c036396d6751a2096bbdc460) |
| Author | Claude |
| Date | 2026-05-14T20:01:23Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Fix iframe-context persona resolution in registry routes

Root cause: services/identity/getActivePersona.ts requires
Authorization: Bearer <jwt> or x-auth-profile-id headers
(personaRepo.ts:210-283). Browser fetch() from the codex iframe
(/triad/embed/codex/...) doesn't auto-attach the Supabase token, so
getActivePersona() returns null. Every persona_owns check then resolves
to false against a populated registry — same symptom regardless of
whether the underlying content_qubes table has 0 or 42 rows.

Fix: new resolveIframePersona(req) helper. Spine-first (cookie /
Authorization header / x-auth-profile-id) and ?personaId= URL-param
fallback for the iframe path. The fallback:
  - verifies the personaId is a UUID and resolves to an active
    personas row before returning a context (random UUIDs don't pass)
  - forces cartridgeFlags.isAdmin and isPartner to false (URL claims
    can NEVER grant admin/partner privileges)
  - defaults identifiability to 'semi_anonymous' floor unless the
    persona row carries a stronger value

Wired into:
  - /api/registry/content-qube/series         (existing route)
  - /api/registry/content-qube/series-rights  (my Phase B endpoint)

NOT wired into:
  - /api/registry/content-qube/browse — admin-gated route must continue
    using getActivePersona strictly so URL claims can't bypass the
    admin gate.

Trust envelope is identical to /api/codex/owned, /api/codex/knyt-purchases,
and /api/entitlements/list, all of which already read personaId directly
from the URL.
```

## Body

Root cause: services/identity/getActivePersona.ts requires
Authorization: Bearer <jwt> or x-auth-profile-id headers
(personaRepo.ts:210-283). Browser fetch() from the codex iframe
(/triad/embed/codex/...) doesn't auto-attach the Supabase token, so
getActivePersona() returns null. Every persona_owns check then resolves
to false against a populated registry — same symptom regardless of
whether the underlying content_qubes table has 0 or 42 rows.

Fix: new resolveIframePersona(req) helper. Spine-first (cookie /
Authorization header / x-auth-profile-id) and ?personaId= URL-param
fallback for the iframe path. The fallback:
  - verifies the personaId is a UUID and resolves to an active
    personas row before returning a context (random UUIDs don't pass)
  - forces cartridgeFlags.isAdmin and isPartner to false (URL claims
    can NEVER grant admin/partner privileges)
  - defaults identifiability to 'semi_anonymous' floor unless the
    persona row carries a stronger value

Wired into:
  - /api/registry/content-qube/series         (existing route)
  - /api/registry/content-qube/series-rights  (my Phase B endpoint)

NOT wired into:
  - /api/registry/content-qube/browse — admin-gated route must continue
    using getActivePersona strictly so URL claims can't bypass the
    admin gate.

Trust envelope is identical to /api/codex/owned, /api/codex/knyt-purchases,
and /api/entitlements/list, all of which already read personaId directly
from the URL.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/registry/content-qube/series-rights/route.ts` |
| Modified | `app/api/registry/content-qube/series/route.ts` |
| Added | `services/identity/resolveIframePersona.ts` |

## Stats

 4 files changed, 138 insertions(+), 8 deletions(-)
