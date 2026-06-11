# Commit Brief: `5261bfc` — Step 2: shared InviteModal + T1 handle resolution server-side

| Field | Value |
|-------|-------|
| SHA | [`5261bfc`](https://github.com/Kn0w-1/AigentZBeta/commit/5261bfc398141fc30dce1a40b6fc50af9fcf87f6) |
| Author | Claude |
| Date | 2026-05-23T09:29:02Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Step 2: shared InviteModal + T1 handle resolution server-side

Step 2 of the share/invite consolidation. Foundation for Steps 4–7.

  • components/shared/InviteModal.tsx (NEW)
    Self-contained invite UI. Replaces MyCanvasTab.InviteBar's
    lifted-state pattern. Takes { isOpen, onClose, entity,
    endpointPath, personaId, onInvited? } and POSTs to endpointPath
    with { invitedHandle, role }. Styled to match aigentMe / remix
    modal chrome: slate-950/80 backdrop, rounded-2xl border-white/10
    card, violet accents on primary actions, X close, Enter to
    submit. Role selector for viewer / commenter.

  • app/api/mycanvas/entries/[id]/invite/route.ts (UPDATED)
    POST handler now accepts EITHER:
      { invitedPersonaId, role }   — legacy T0 path (back-compat)
      { invitedHandle, role }      — preferred T1 path
    When invitedHandle is provided, resolveHandleToPersonaId()
    queries personas (by UUID, did:iq:<hex>, fio_handle, evm_address)
    and agent_keys (by fio_handle) — same matrix as
    /api/identity/resolve-recipient but returns persona_id (T0)
    server-side instead of evm_address. The persona_id then flows
    into mycanvas_invites unchanged. Browser never sees the T0 id.

Net: clients send handles, server resolves to persona_id, T0 stays
T0. No browser-bound persona_id leak in the invite flow.

Doesn't touch MyCanvasTab yet — that's Step 5. The legacy
invitedPersonaId path stays alive so the existing InviteBar keeps
working during the migration.
```

## Body

Step 2 of the share/invite consolidation. Foundation for Steps 4–7.

  • components/shared/InviteModal.tsx (NEW)
    Self-contained invite UI. Replaces MyCanvasTab.InviteBar's
    lifted-state pattern. Takes { isOpen, onClose, entity,
    endpointPath, personaId, onInvited? } and POSTs to endpointPath
    with { invitedHandle, role }. Styled to match aigentMe / remix
    modal chrome: slate-950/80 backdrop, rounded-2xl border-white/10
    card, violet accents on primary actions, X close, Enter to
    submit. Role selector for viewer / commenter.

  • app/api/mycanvas/entries/[id]/invite/route.ts (UPDATED)
    POST handler now accepts EITHER:
      { invitedPersonaId, role }   — legacy T0 path (back-compat)
      { invitedHandle, role }      — preferred T1 path
    When invitedHandle is provided, resolveHandleToPersonaId()
    queries personas (by UUID, did:iq:<hex>, fio_handle, evm_address)
    and agent_keys (by fio_handle) — same matrix as
    /api/identity/resolve-recipient but returns persona_id (T0)
    server-side instead of evm_address. The persona_id then flows
    into mycanvas_invites unchanged. Browser never sees the T0 id.

Net: clients send handles, server resolves to persona_id, T0 stays
T0. No browser-bound persona_id leak in the invite flow.

Doesn't touch MyCanvasTab yet — that's Step 5. The legacy
invitedPersonaId path stays alive so the existing InviteBar keeps
working during the migration.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/mycanvas/entries/[id]/invite/route.ts` |
| Added | `components/shared/InviteModal.tsx` |

## Stats

 2 files changed, 337 insertions(+), 5 deletions(-)
