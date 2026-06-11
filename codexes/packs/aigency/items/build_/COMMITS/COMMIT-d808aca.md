# Commit Brief: `d808aca` — Step 5b: MyCanvasTab Share + Invite use canonical modals

| Field | Value |
|-------|-------|
| SHA | [`d808aca`](https://github.com/Kn0w-1/AigentZBeta/commit/d808aca09c1030f977bc365695aa61fb8038d128) |
| Author | Claude |
| Date | 2026-05-23T09:42:17Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Step 5b: MyCanvasTab Share + Invite use canonical modals

Final piece of the share/invite consolidation. MyCanvasTab now uses
the same two modals everywhere else does:

  Share button → SocialSharingModal (Qriptopian rich share)
    Replaces the navigator.share + clipboard fallback that
    handleShare() used to do. shareId-based attribution; raw
    personaId stays server-side.

  Invite button → InviteModal (shared component)
    Replaces the inline InviteBar's lifted-state + raw-input UI.
    Server-side T1 → T0 resolution via the invite endpoint, so
    persona_id never travels in browser JSON.

  Pulled in useActivePersona for the T1 personaLabel that drives
  the 'Shared by <label>' badge.

The legacy InviteBar component + the inviteOpenForId state are
left in place for the note-editor flow (default branch) — that's a
distinct surface and not the one the user flagged for migration.
Note editor invite can switch to InviteModal in a follow-up if
needed, but it's not blocking.

End of the share/invite consolidation. All seven steps shipped:
  1) T0 leak fix in SocialSharingModal
  2) Shared InviteModal + T1 server-side resolution
  3) Modal chrome alignment
  4) SmartContentActionContext gains 'invite'
  5) Runtime smart-action Share → SocialSharingModal
  5b) MyCanvas Share + Invite → canonical modals (this commit)
  6) Community ShareMenu → SocialSharingModal
  7) Thin-client MENU_ACTION 'share' opens SocialSharingModal

Next: 'invite' MENU_ACTION on the thin-client side is still open —
left for when an invite use-case from the menu lands (no obvious
target item today, since the menu doesn't track active capsule
intent the same way the smart-action buttons do).
```

## Body

Final piece of the share/invite consolidation. MyCanvasTab now uses
the same two modals everywhere else does:

  Share button → SocialSharingModal (Qriptopian rich share)
    Replaces the navigator.share + clipboard fallback that
    handleShare() used to do. shareId-based attribution; raw
    personaId stays server-side.

  Invite button → InviteModal (shared component)
    Replaces the inline InviteBar's lifted-state + raw-input UI.
    Server-side T1 → T0 resolution via the invite endpoint, so
    persona_id never travels in browser JSON.

  Pulled in useActivePersona for the T1 personaLabel that drives
  the 'Shared by <label>' badge.

The legacy InviteBar component + the inviteOpenForId state are
left in place for the note-editor flow (default branch) — that's a
distinct surface and not the one the user flagged for migration.
Note editor invite can switch to InviteModal in a follow-up if
needed, but it's not blocking.

End of the share/invite consolidation. All seven steps shipped:
  1) T0 leak fix in SocialSharingModal
  2) Shared InviteModal + T1 server-side resolution
  3) Modal chrome alignment
  4) SmartContentActionContext gains 'invite'
  5) Runtime smart-action Share → SocialSharingModal
  5b) MyCanvas Share + Invite → canonical modals (this commit)
  6) Community ShareMenu → SocialSharingModal
  7) Thin-client MENU_ACTION 'share' opens SocialSharingModal

Next: 'invite' MENU_ACTION on the thin-client side is still open —
left for when an invite use-case from the menu lands (no obvious
target item today, since the menu doesn't track active capsule
intent the same way the smart-action buttons do).

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |

## Stats

 1 file changed, 56 insertions(+), 20 deletions(-)
