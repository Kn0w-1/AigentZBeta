# Commit Brief: `4d430c2` — add knyt cartridge Quests tab — canonical task library

| Field | Value |
|-------|-------|
| SHA | [`4d430c2`](https://github.com/Kn0w-1/AigentZBeta/commit/4d430c2ab21efda6f36c9ffe59243530994331fd) |
| Author | Claude |
| Date | 2026-05-19T23:17:58Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
add knyt cartridge Quests tab — canonical task library

New top-level KNYT Cartridge tab ("Quests", order 3.5, between Order
group and 21 Sats) that serves as the single canonical home for all
four KNYT task families:

- Bring a Knight — referral ladder, reward model, points to wallet
  for share-link issuance.
- Knight of Attention — episode + streak rewards, deep-links to
  Scrolls.
- Herald of the Order — share-attribution ladder (click → signup →
  conversion), points to wallet for Herald share link.
- Living Canon — 21 Sats — three rung CTAs (vote / submit / dispatch)
  plus all 9 archetype chips, each deep-linking to the 21 Sats tab
  with the matching submission schema preloaded.

The tab is generic + explanatory by design — personal task progress,
claimable rewards, and next-best action stay on the Order tab and
wallet drawer (those are the user-summary surfaces). This gives Bring
a Knight / Knight of Attention / Herald their first cartridge-level
explanation surface, which previously only existed in the wallet.

Wired through the existing TabRenderer componentRegistry and codex-
configs tab list. Cross-tab navigation uses tryOpenInMountedCartridge
+ window-event dispatch, matching the wallet drawer's existing
navigateToKnytTab pattern (SmartWalletDrawer.tsx:1528).
```

## Body

New top-level KNYT Cartridge tab ("Quests", order 3.5, between Order
group and 21 Sats) that serves as the single canonical home for all
four KNYT task families:

- Bring a Knight — referral ladder, reward model, points to wallet
  for share-link issuance.
- Knight of Attention — episode + streak rewards, deep-links to
  Scrolls.
- Herald of the Order — share-attribution ladder (click → signup →
  conversion), points to wallet for Herald share link.
- Living Canon — 21 Sats — three rung CTAs (vote / submit / dispatch)
  plus all 9 archetype chips, each deep-linking to the 21 Sats tab
  with the matching submission schema preloaded.

The tab is generic + explanatory by design — personal task progress,
claimable rewards, and next-best action stay on the Order tab and
wallet drawer (those are the user-summary surfaces). This gives Bring
a Knight / Knight of Attention / Herald their first cartridge-level
explanation surface, which previously only existed in the wallet.

Wired through the existing TabRenderer componentRegistry and codex-
configs tab list. Cross-tab navigation uses tryOpenInMountedCartridge
+ window-event dispatch, matching the wallet drawer's existing
navigateToKnytTab pattern (SmartWalletDrawer.tsx:1528).

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/TabRenderer.tsx` |
| Added | `app/triad/components/codex/tabs/KnytQuestsTab.tsx` |
| Modified | `data/codex-configs.ts` |

## Stats

 3 files changed, 312 insertions(+)
