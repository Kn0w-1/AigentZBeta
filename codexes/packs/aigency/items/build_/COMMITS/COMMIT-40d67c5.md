# Commit Brief: `40d67c5` — share modal: stop leaking personaId in URL + badge (CLAUDE.md PARAMOUNT)

| Field | Value |
|-------|-------|
| SHA | [`40d67c5`](https://github.com/Kn0w-1/AigentZBeta/commit/40d67c516558ad803049da79d9754ebd5d87ca12) |
| Author | Claude |
| Date | 2026-05-23T09:09:34Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
share modal: stop leaking personaId in URL + badge (CLAUDE.md PARAMOUNT)

Privacy violation found while planning the share/invite consolidation.
SocialSharingModal embedded personaId in two places, both of which
appear in browser-bound surfaces:

  • Visible UI badge: 'Shared via persona: <UUID>' (the raw T0 id)
  • Deep link URL query: ?persona=<UUID> — propagated to every social
    post and any tracking pixel that reads the URL

Both violate CLAUDE.md § Identity & Access Spine's PARAMOUNT rule:
personaId is T0 server-internal and must NEVER appear in browser-
bound JSON or chain-bound receipts (along with authProfileId,
rootDid, kybeAttestation, cross-persona fioHandle).

Fix:
  • personaId stays a prop but is used ONLY server-side, via the
    new useEffect that POSTs to /api/social/track on modal open
    to register the shareId → persona_id mapping in the existing
    social_share_analytics table. Attribution now lives in the
    server-side mapping, not in the URL.
  • personaId is no longer set as a query param on the deep link.
  • New personaLabel prop (T1) drives the visible badge — 'Shared
    by <label>'. Falls back to no badge when not supplied rather
    than ever falling back to the UUID.

Server attribution still works end-to-end:
  modal mount → POST /track {shareId, personaId, contentId, eventType:'create'}
    → social_share_analytics row created with persona_id
  link click → GET /track?s=<shareId>&r=<url>
    → counter increments, persona credit resolved server-side

Step 1 of the consolidation plan. Next steps (InviteModal extraction,
SocialSharingModal styling alignment, SmartContentActionContext
'invite' handler, MyCanvas + Community migration, thin-client menu
wiring) follow as separate commits per the user-approved plan.
```

## Body

Privacy violation found while planning the share/invite consolidation.
SocialSharingModal embedded personaId in two places, both of which
appear in browser-bound surfaces:

  • Visible UI badge: 'Shared via persona: <UUID>' (the raw T0 id)
  • Deep link URL query: ?persona=<UUID> — propagated to every social
    post and any tracking pixel that reads the URL

Both violate CLAUDE.md § Identity & Access Spine's PARAMOUNT rule:
personaId is T0 server-internal and must NEVER appear in browser-
bound JSON or chain-bound receipts (along with authProfileId,
rootDid, kybeAttestation, cross-persona fioHandle).

Fix:
  • personaId stays a prop but is used ONLY server-side, via the
    new useEffect that POSTs to /api/social/track on modal open
    to register the shareId → persona_id mapping in the existing
    social_share_analytics table. Attribution now lives in the
    server-side mapping, not in the URL.
  • personaId is no longer set as a query param on the deep link.
  • New personaLabel prop (T1) drives the visible badge — 'Shared
    by <label>'. Falls back to no badge when not supplied rather
    than ever falling back to the UUID.

Server attribution still works end-to-end:
  modal mount → POST /track {shareId, personaId, contentId, eventType:'create'}
    → social_share_analytics row created with persona_id
  link click → GET /track?s=<shareId>&r=<url>
    → counter increments, persona credit resolved server-side

Step 1 of the consolidation plan. Next steps (InviteModal extraction,
SocialSharingModal styling alignment, SmartContentActionContext
'invite' handler, MyCanvas + Community migration, thin-client menu
wiring) follow as separate commits per the user-approved plan.

## Files Changed

| Change | File |
|--------|------|
| Modified | `packages/smarttriad/src/SocialSharingModal.tsx` |

## Stats

 1 file changed, 59 insertions(+), 17 deletions(-)
