# Commit Brief: `95f4150` — Path B (composition): personaFetch attaches x-persona-id from localStorage

| Field | Value |
|-------|-------|
| SHA | [`95f4150`](https://github.com/Kn0w-1/AigentZBeta/commit/95f41506a150cdbc47f67d28c6409fd99c987e0d) |
| Author | Claude |
| Date | 2026-05-22T23:10:39Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Path B (composition): personaFetch attaches x-persona-id from localStorage

ROOT CAUSE confirmed by /api/admin/diag/persona-resolution dump:
  • personas_active_asc[0] = devagent@qripto, created_at
    2025-12-09T03:49:10.200549+00:00 (~1 minute older than
    aigentz@aigent at 03:50:14)
  • resolver_output.source = 'session-cookie' (step 4 / default fired)
  • hint: 'resolver picked personas_active_asc[0] — step 4 (default
    first-owned) is firing'

So devagent's row really is the genuine oldest by created_at — all
Dec 9 personas were seeded together during initial dev and devagent
landed milliseconds earlier than the others. The user's mental model
of 'devagent was last' came from recently assigning admin rights
(cartridgeFlags.isAdmin: true on the same row), but the row itself
predates everything.

FIX — pure composition, no protected file modified.

PersonaContext.setActivePersonaId already writes the user's choice to
localStorage['currentPersonaId'] (line 309). All we need is for
personaFetch to read it and forward as an x-persona-id header on every
request. getActivePersona's priority 2 already honours this header
(line 102 of the resolver) AND validates ownership before accepting
it (it rejects non-owned ids), so it's ownership-safe.

After this lands:
  • Every persona switch in the wallet drawer → instant effect on the
    next request, because every request carries x-persona-id.
  • getActivePersona never falls to step 4 for users with a chosen
    persona in localStorage.
  • Brand-new users (no localStorage entry yet) still hit step 4 and
    get devagent first time — that's the next surgical follow-up
    (default_persona_id column on auth_profiles, set by the wallet
    drawer's switch action — Path B-2). Not blocking today's user
    fix; logging proposal in the next message.

Zero changes to getActivePersona.ts (CLAUDE.md protected file).
Zero schema changes. Zero new endpoints. ~15 LOC in personaFetch.
```

## Body

ROOT CAUSE confirmed by /api/admin/diag/persona-resolution dump:
  • personas_active_asc[0] = devagent@qripto, created_at
    2025-12-09T03:49:10.200549+00:00 (~1 minute older than
    aigentz@aigent at 03:50:14)
  • resolver_output.source = 'session-cookie' (step 4 / default fired)
  • hint: 'resolver picked personas_active_asc[0] — step 4 (default
    first-owned) is firing'

So devagent's row really is the genuine oldest by created_at — all
Dec 9 personas were seeded together during initial dev and devagent
landed milliseconds earlier than the others. The user's mental model
of 'devagent was last' came from recently assigning admin rights
(cartridgeFlags.isAdmin: true on the same row), but the row itself
predates everything.

FIX — pure composition, no protected file modified.

PersonaContext.setActivePersonaId already writes the user's choice to
localStorage['currentPersonaId'] (line 309). All we need is for
personaFetch to read it and forward as an x-persona-id header on every
request. getActivePersona's priority 2 already honours this header
(line 102 of the resolver) AND validates ownership before accepting
it (it rejects non-owned ids), so it's ownership-safe.

After this lands:
  • Every persona switch in the wallet drawer → instant effect on the
    next request, because every request carries x-persona-id.
  • getActivePersona never falls to step 4 for users with a chosen
    persona in localStorage.
  • Brand-new users (no localStorage entry yet) still hit step 4 and
    get devagent first time — that's the next surgical follow-up
    (default_persona_id column on auth_profiles, set by the wallet
    drawer's switch action — Path B-2). Not blocking today's user
    fix; logging proposal in the next message.

Zero changes to getActivePersona.ts (CLAUDE.md protected file).
Zero schema changes. Zero new endpoints. ~15 LOC in personaFetch.

## Files Changed

| Change | File |
|--------|------|
| Modified | `utils/personaSpine.tsx` |

## Stats

 1 file changed, 21 insertions(+)
