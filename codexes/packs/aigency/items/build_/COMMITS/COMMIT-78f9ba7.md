# Commit Brief: `78f9ba7` — Steps 6+7: community ShareMenu uses SocialSharingModal; thin-client 'share' MENU_ACTION opens it

| Field | Value |
|-------|-------|
| SHA | [`78f9ba7`](https://github.com/Kn0w-1/AigentZBeta/commit/78f9ba79d52e444dc343d2f229e0f3a0c5fdf242) |
| Author | Claude |
| Date | 2026-05-23T09:40:10Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Steps 6+7: community ShareMenu uses SocialSharingModal; thin-client 'share' MENU_ACTION opens it

Step 6 — Community tab:
  • Replaced the 3-option dropdown (Copy / X / Email) in
    KnytCommunityContentTab.ShareMenu with the canonical Qriptopian
    SocialSharingModal — same modal as the runtime smart-action
    Share button now uses (Step 5).
  • ShareMenu now takes { item, personaId, personaLabel } and the
    button opens the modal pre-populated with the community item's
    title/skill and a community-content/<id> URL.
  • ContentDetail reads useActivePersona() for the T1 displayLabel
    (or ownFioHandle) and threads it through. Raw personaId stays
    server-side only — used by /api/social/track for attribution.

Step 7 — Thin-client MENU_ACTION:
  • Added 'share' handler to DRAWER_ACTION_HANDLERS in the runtime's
    MENU_ACTION receiver. When the thin-client shell sends
    { action_id: 'share' }, we now open SocialSharingModal with the
    active capsule pre-populated (falls back to first available
    capsule or a generic 'metaMe' item when no capsule is active).
  • 'share-refer' kept distinct — that's the 'invite a friend to the
    platform' refer flow, native-share / clipboard fallback only.

Steps 6 + 7 of the share/invite consolidation. Remaining:
  Step 5b — MyCanvasTab Share + Invite (lift its inline InviteBar
    into the shared InviteModal; use SocialSharingModal for share).
  Step 7b — Lovable thin-client mirror — instruction already in
    Lovable's queue; the platform shell action handlers + the
    runtime-side handler shipped here are the receivers, all
    Lovable needs to do is dispatch action_id: 'share' or 'invite'
    with the current item context.
```

## Body

Step 6 — Community tab:
  • Replaced the 3-option dropdown (Copy / X / Email) in
    KnytCommunityContentTab.ShareMenu with the canonical Qriptopian
    SocialSharingModal — same modal as the runtime smart-action
    Share button now uses (Step 5).
  • ShareMenu now takes { item, personaId, personaLabel } and the
    button opens the modal pre-populated with the community item's
    title/skill and a community-content/<id> URL.
  • ContentDetail reads useActivePersona() for the T1 displayLabel
    (or ownFioHandle) and threads it through. Raw personaId stays
    server-side only — used by /api/social/track for attribution.

Step 7 — Thin-client MENU_ACTION:
  • Added 'share' handler to DRAWER_ACTION_HANDLERS in the runtime's
    MENU_ACTION receiver. When the thin-client shell sends
    { action_id: 'share' }, we now open SocialSharingModal with the
    active capsule pre-populated (falls back to first available
    capsule or a generic 'metaMe' item when no capsule is active).
  • 'share-refer' kept distinct — that's the 'invite a friend to the
    platform' refer flow, native-share / clipboard fallback only.

Steps 6 + 7 of the share/invite consolidation. Remaining:
  Step 5b — MyCanvasTab Share + Invite (lift its inline InviteBar
    into the shared InviteModal; use SocialSharingModal for share).
  Step 7b — Lovable thin-client mirror — instruction already in
    Lovable's queue; the platform shell action handlers + the
    runtime-side handler shipped here are the receivers, all
    Lovable needs to do is dispatch action_id: 'share' or 'invite'
    with the current item context.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytCommunityContentTab.tsx` |
| Modified | `components/metame/MetaMeRuntimeClient.tsx` |

## Stats

 2 files changed, 66 insertions(+), 56 deletions(-)
