# Commit Brief: `50d7642` — Step 4: SmartContentActionContext gains 'invite' handler — opens shared InviteModal

| Field | Value |
|-------|-------|
| SHA | [`50d7642`](https://github.com/Kn0w-1/AigentZBeta/commit/50d7642268658f2754839782fa9477594e9a12fe) |
| Author | Claude |
| Date | 2026-05-23T09:34:08Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Step 4: SmartContentActionContext gains 'invite' handler — opens shared InviteModal

  • packages/smarttriad/src/types.ts: ActionType extended with 'invite'
    so the context's typed handler accepts it without a cast.
  • app/contexts/SmartContentActionContext.tsx:
    - imports InviteModal from components/shared
    - new state inviteModalOpen + inviteItem
    - new case 'invite' in executeAction: stash item, open modal
    - mounts <InviteModal> globally alongside the SocialSharingModal
      so any surface using executeAction('invite', item) gets the
      canonical UI.

Endpoint targets /api/mycanvas/entries/[id]/invite today — the only
invite-bearing table in the platform. Broader routing (content qube
invites, capsule invites, etc.) is a one-line addition when those
tables land: replace the inline endpointPath template with an
inviteEndpointFor(item) resolver.

Step 4 of the share/invite consolidation. Next:
  Step 5 — migrate MyCanvasTab share/invite buttons through context
  Step 6 — migrate Community tab share through context
  Step 7 — wire thin-client MENU_ACTION share + invite to context
```

## Body

• packages/smarttriad/src/types.ts: ActionType extended with 'invite'
    so the context's typed handler accepts it without a cast.
  • app/contexts/SmartContentActionContext.tsx:
    - imports InviteModal from components/shared
    - new state inviteModalOpen + inviteItem
    - new case 'invite' in executeAction: stash item, open modal
    - mounts <InviteModal> globally alongside the SocialSharingModal
      so any surface using executeAction('invite', item) gets the
      canonical UI.

Endpoint targets /api/mycanvas/entries/[id]/invite today — the only
invite-bearing table in the platform. Broader routing (content qube
invites, capsule invites, etc.) is a one-line addition when those
tables land: replace the inline endpointPath template with an
inviteEndpointFor(item) resolver.

Step 4 of the share/invite consolidation. Next:
  Step 5 — migrate MyCanvasTab share/invite buttons through context
  Step 6 — migrate Community tab share through context
  Step 7 — wire thin-client MENU_ACTION share + invite to context

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/contexts/SmartContentActionContext.tsx` |
| Modified | `packages/smarttriad/src/types.ts` |

## Stats

 2 files changed, 40 insertions(+), 1 deletion(-)
