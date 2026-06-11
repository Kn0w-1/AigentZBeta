# Commit Brief: `935546e` — Path A persona-resolution defense: PST header on /generate, refuse submit without active persona

| Field | Value |
|-------|-------|
| SHA | [`935546e`](https://github.com/Kn0w-1/AigentZBeta/commit/935546e9725e6dea7c19a16f7ecee3693cd4e63d) |
| Author | Claude |
| Date | 2026-05-22T20:21:08Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Path A persona-resolution defense: PST header on /generate, refuse submit without active persona

User confirmed: devagent is actually the MOST recently created persona,
so the server's 'first owned by created_at ASC' fallback shouldn't even
be picking it — yet it does. Either the timestamps were back-filled at
migration time, or something inverts the sort for this auth profile.
Either way, the silent fallback is the bug — the platform should never
guess.

Path A (this commit) — client-side defense in RemixDialog:

  1) Refuse to submit if BOTH personaId prop AND PST surface are null
     — surfaces the upstream problem (which persona is active?) instead
     of letting the server silently default to the wrong row. The user
     sees 'No active persona resolved — please pick a persona in the
     wallet drawer and retry.'

  2) Attach the persona-session-token from useActivePersona().surface
     as the x-persona-session-token request header on /generate. This
     trips priority 1 of getActivePersona() (PST bound to a specific
     persona) instead of falling through to priority 4 (silent default).
     Priority 1 also enforces auth-profile binding, so it can't be
     replayed across sessions.

Path B (added to backlog brief, NOT shipped today):

  Server-side schema + resolver change:
    • ALTER TABLE auth_profiles ADD COLUMN default_persona_id
    • New priority 3.5 in getActivePersona: auth_profiles.default_persona_id
      (when owned)
    • Step 4 (last-resort default) flips to created_at DESC so first-time
      users get their MOST recently created persona, not the oldest
    • Wallet drawer's setActivePersonaId() persists to a new
      /api/wallet/persona/set-default endpoint
    • Migration: backfill default_persona_id from qc_transactions /
      orchestration_events last-active-by-persona
    • Phased rollout behind a feature flag, dele tested first

  getActivePersona.ts is in CLAUDE.md's protected files list, so Path B
  needs operator approval before the resolver itself changes. The
  schema + migration + new endpoint can land independently in advance.

Updated codexes/packs/agentiq/updates/
  2026-05-22_qc-dvn-mainnet-parity-backlog.md with the full Path B
  design so it stays visible alongside the parity workstream.
```

## Body

User confirmed: devagent is actually the MOST recently created persona,
so the server's 'first owned by created_at ASC' fallback shouldn't even
be picking it — yet it does. Either the timestamps were back-filled at
migration time, or something inverts the sort for this auth profile.
Either way, the silent fallback is the bug — the platform should never
guess.

Path A (this commit) — client-side defense in RemixDialog:

  1) Refuse to submit if BOTH personaId prop AND PST surface are null
     — surfaces the upstream problem (which persona is active?) instead
     of letting the server silently default to the wrong row. The user
     sees 'No active persona resolved — please pick a persona in the
     wallet drawer and retry.'

  2) Attach the persona-session-token from useActivePersona().surface
     as the x-persona-session-token request header on /generate. This
     trips priority 1 of getActivePersona() (PST bound to a specific
     persona) instead of falling through to priority 4 (silent default).
     Priority 1 also enforces auth-profile binding, so it can't be
     replayed across sessions.

Path B (added to backlog brief, NOT shipped today):

  Server-side schema + resolver change:
    • ALTER TABLE auth_profiles ADD COLUMN default_persona_id
    • New priority 3.5 in getActivePersona: auth_profiles.default_persona_id
      (when owned)
    • Step 4 (last-resort default) flips to created_at DESC so first-time
      users get their MOST recently created persona, not the oldest
    • Wallet drawer's setActivePersonaId() persists to a new
      /api/wallet/persona/set-default endpoint
    • Migration: backfill default_persona_id from qc_transactions /
      orchestration_events last-active-by-persona
    • Phased rollout behind a feature flag, dele tested first

  getActivePersona.ts is in CLAUDE.md's protected files list, so Path B
  needs operator approval before the resolver itself changes. The
  schema + migration + new endpoint can land independently in advance.

Updated codexes/packs/agentiq/updates/
  2026-05-22_qc-dvn-mainnet-parity-backlog.md with the full Path B
  design so it stays visible alongside the parity workstream.

## Files Changed

| Change | File |
|--------|------|
| Modified | `codexes/packs/agentiq/updates/2026-05-22_qc-dvn-mainnet-parity-backlog.md` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 2 files changed, 102 insertions(+), 3 deletions(-)
