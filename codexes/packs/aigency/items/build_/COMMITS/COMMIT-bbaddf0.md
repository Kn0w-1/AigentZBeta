# Commit Brief: `bbaddf0` — Fix resolveIframePersona to accept FIO handles; add ownedIssues fetch logging

| Field | Value |
|-------|-------|
| SHA | [`bbaddf0`](https://github.com/Kn0w-1/AigentZBeta/commit/bbaddf0bfd1fa9278ba6c7bca3260e9fff61a37a) |
| Author | Claude |
| Date | 2026-05-15T00:06:19Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Fix resolveIframePersona to accept FIO handles; add ownedIssues fetch logging

resolveIframePersona only accepted UUID-form personaId values (UUID_RE test).
KnytTab's effectivePersonaId can be a FIO handle (e.g. arkagent@knyt) when
activePersonaId from PersonaContext resolves before the UUID prop arrives.
This caused series-rights to get null persona context → all persona_owns=false
→ registryOwnership empty → cards routed to paywall.

Fix mirrors the handle resolution already in /api/codex/owned (line 83-90):
if personaId contains '@', look up by fio_handle instead of id.

Also adds console.warn on non-ok /api/codex/owned response (was silently
discarded) and console.log for issueCount on success, so the root cause of
ownedCount=0 cross-check logs is diagnosable from the browser console.
```

## Body

resolveIframePersona only accepted UUID-form personaId values (UUID_RE test).
KnytTab's effectivePersonaId can be a FIO handle (e.g. arkagent@knyt) when
activePersonaId from PersonaContext resolves before the UUID prop arrives.
This caused series-rights to get null persona context → all persona_owns=false
→ registryOwnership empty → cards routed to paywall.

Fix mirrors the handle resolution already in /api/codex/owned (line 83-90):
if personaId contains '@', look up by fio_handle instead of id.

Also adds console.warn on non-ok /api/codex/owned response (was silently
discarded) and console.log for issueCount on success, so the root cause of
ownedCount=0 cross-check logs is diagnosable from the browser console.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |
| Modified | `services/identity/resolveIframePersona.ts` |

## Stats

 2 files changed, 22 insertions(+), 2 deletions(-)
