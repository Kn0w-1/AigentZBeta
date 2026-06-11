# Commit Brief: `75a0c82` — SmartTriadProvider/Surfaces: openInvite parity + personaLabel on share modal

| Field | Value |
|-------|-------|
| SHA | [`75a0c82`](https://github.com/Kn0w-1/AigentZBeta/commit/75a0c822135e85ec999a7d91e1c8c0220a490f43) |
| Author | Claude |
| Date | 2026-05-23T10:49:44Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
SmartTriadProvider/Surfaces: openInvite parity + personaLabel on share modal

Two surface improvements that fix gaps in the share/invite consolidation:

1) SmartTriadProvider now exposes openInvite / closeInvite alongside
   openShare / closeShare. State carries inviteItem the same way it
   carries shareItem so any cartridge surface that calls
   actions.openShare(item) can also call actions.openInvite(item) and
   get the canonical InviteModal — no shape conversion required (both
   take ShareItem).

2) SmartTriadSurfaces mounts the InviteModal next to the
   SocialSharingModal so once a surface calls actions.openInvite,
   the modal appears globally. Targets
   /api/mycanvas/entries/<id>/invite today (the only invite-bearing
   table). When other entity invite tables ship, extend the
   endpointPath resolver here.

3) Threads personaLabel from useActivePersona() through to
   SocialSharingModal so the 'Shared by <label>' badge actually
   renders for cartridge tab shares (previously only the runtime
   surface had the label wired; cartridge tabs were leaving the
   badge empty).

Surfaces that already call actions.openShare (FeaturesTab,
QriptoScrollsTab, KnytTab, Kn0wdZTab, QriptoLiquidCodexTab,
PennyDropsTab) now get the badge automatically. Adding invite
buttons in those rows is a one-line caller change:
  onInvite={() => actions.openInvite(item)}
Will sweep through them as the cartridges add invite affordances.
```

## Body

Two surface improvements that fix gaps in the share/invite consolidation:

1) SmartTriadProvider now exposes openInvite / closeInvite alongside
   openShare / closeShare. State carries inviteItem the same way it
   carries shareItem so any cartridge surface that calls
   actions.openShare(item) can also call actions.openInvite(item) and
   get the canonical InviteModal — no shape conversion required (both
   take ShareItem).

2) SmartTriadSurfaces mounts the InviteModal next to the
   SocialSharingModal so once a surface calls actions.openInvite,
   the modal appears globally. Targets
   /api/mycanvas/entries/<id>/invite today (the only invite-bearing
   table). When other entity invite tables ship, extend the
   endpointPath resolver here.

3) Threads personaLabel from useActivePersona() through to
   SocialSharingModal so the 'Shared by <label>' badge actually
   renders for cartridge tab shares (previously only the runtime
   surface had the label wired; cartridge tabs were leaving the
   badge empty).

Surfaces that already call actions.openShare (FeaturesTab,
QriptoScrollsTab, KnytTab, Kn0wdZTab, QriptoLiquidCodexTab,
PennyDropsTab) now get the badge automatically. Adding invite
buttons in those rows is a one-line caller change:
  onInvite={() => actions.openInvite(item)}
Will sweep through them as the cartridges add invite affordances.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/components/content/SmartTriadProvider.tsx` |
| Modified | `app/components/content/SmartTriadSurfaces.tsx` |

## Stats

 2 files changed, 51 insertions(+)
