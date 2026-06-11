# Commit Brief: `7f0169e` — aigentMe welcome: fix lore-flavour smoking gun + move request-access chip to right pane

| Field | Value |
|-------|-------|
| SHA | [`7f0169e`](https://github.com/Kn0w-1/AigentZBeta/commit/7f0169eef33b09dc243613e4750919852d3a3551) |
| Author | Claude |
| Date | 2026-05-26T19:20:25Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
aigentMe welcome: fix lore-flavour smoking gun + move request-access chip to right pane

Four fixes from the post-Phase-E feedback pass.

#7 Smoking gun: 'SmartTriad-key-term">metaKnyts universe' garbled
output. Root cause was self-collision in processInlineFormatting —
the case-insensitive 'SmartTriad' regex matched the 'smarttriad'
inside class="smarttriad-key-term" attributes inserted by an
earlier iteration (e.g. metaKnyts → wrapped span → 'SmartTriad'
loop then sees 'smarttriad' inside the class attribute and tries
to wrap it). Replaced the loop with placeholder-then-expand using
NUL-bracketed markers so later passes can't re-match inside the
wrapper spans.

#7 secondary: lore-flavoured responses on the aigentMe surface even
when KNYT cartridge was active. Root cause was SmartTriadCopilotLayer
treating the spine persona UUID as an agent identifier. Line was:
  const resolvedPersona = personaId ?? agent?.id ?? 'aigent-z';
When personaId is a UUID like 'info2knyt-...', it doesn't start with
'aigent-' so the chat route's defaultAgentIdForPersona() falls back
to 'aigent-kn0w1' — Kn0w1's persona prompt then drives the response
('passionate story enthusiast in the metaKnyts universe…'). Rewrote
the resolver to prefer agent.id, only accept personaId as an agent
id when it actually starts with 'aigent-', and default to aigent-z
otherwise. Spine persona UUID is still forwarded separately as
body.personaId for live-context lookups.

#2 Insert-failed error: surface the Postgres detail back to the
modal so the operator can act on it. Added graceful fallback when
migration 20260526020000 (request_type column) hasn't been applied
yet — retry the insert without request_type so the alpha workflow
keeps working on environments behind on migrations. Response now
carries error.message + error.code instead of an opaque
'insert-failed' string.

#3 + #4 Move pulse pill into right-pane badge carousel alongside
ExpGuide / PersonaQube. Built RequestAccessChip self-contained
inside WelcomeRightPane: pulses, dismisses via × button (sessionStorage),
mounts the request modal in controlled mode. Modal now defaults to
the FIRST cartridge the persona doesn't already have active —
no more 'why am I requesting access to metaMe?' confusion when
sitting on the metame surface. WelcomeRightPane Props gained
isGlobalAdmin, hasCartridgeAdminGrant, activeCartridges. Threaded
from AigentMeWelcomeSplitTab via adminGrants + the existing
activeCartridges derivation.
```

## Body

Four fixes from the post-Phase-E feedback pass.

#7 Smoking gun: 'SmartTriad-key-term">metaKnyts universe' garbled
output. Root cause was self-collision in processInlineFormatting —
the case-insensitive 'SmartTriad' regex matched the 'smarttriad'
inside class="smarttriad-key-term" attributes inserted by an
earlier iteration (e.g. metaKnyts → wrapped span → 'SmartTriad'
loop then sees 'smarttriad' inside the class attribute and tries
to wrap it). Replaced the loop with placeholder-then-expand using
NUL-bracketed markers so later passes can't re-match inside the
wrapper spans.

#7 secondary: lore-flavoured responses on the aigentMe surface even
when KNYT cartridge was active. Root cause was SmartTriadCopilotLayer
treating the spine persona UUID as an agent identifier. Line was:
  const resolvedPersona = personaId ?? agent?.id ?? 'aigent-z';
When personaId is a UUID like 'info2knyt-...', it doesn't start with
'aigent-' so the chat route's defaultAgentIdForPersona() falls back
to 'aigent-kn0w1' — Kn0w1's persona prompt then drives the response
('passionate story enthusiast in the metaKnyts universe…'). Rewrote
the resolver to prefer agent.id, only accept personaId as an agent
id when it actually starts with 'aigent-', and default to aigent-z
otherwise. Spine persona UUID is still forwarded separately as
body.personaId for live-context lookups.

#2 Insert-failed error: surface the Postgres detail back to the
modal so the operator can act on it. Added graceful fallback when
migration 20260526020000 (request_type column) hasn't been applied
yet — retry the insert without request_type so the alpha workflow
keeps working on environments behind on migrations. Response now
carries error.message + error.code instead of an opaque
'insert-failed' string.

#3 + #4 Move pulse pill into right-pane badge carousel alongside
ExpGuide / PersonaQube. Built RequestAccessChip self-contained
inside WelcomeRightPane: pulses, dismisses via × button (sessionStorage),
mounts the request modal in controlled mode. Modal now defaults to
the FIRST cartridge the persona doesn't already have active —
no more 'why am I requesting access to metaMe?' confusion when
sitting on the metame surface. WelcomeRightPane Props gained
isGlobalAdmin, hasCartridgeAdminGrant, activeCartridges. Threaded
from AigentMeWelcomeSplitTab via adminGrants + the existing
activeCartridges derivation.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/admin/access-requests/route.ts` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |
| Modified | `components/smarttriad/copilot/SmartTriadCopilotLayer.tsx` |
| Modified | `components/smarttriad/copilot/SmartTriadInferenceRenderer.tsx` |

## Stats

 5 files changed, 214 insertions(+), 51 deletions(-)
