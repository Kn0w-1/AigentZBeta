# Commit Brief: `210c740` — 4-tier nav row + Marketa Admin in metaMe + Studio Admin stub + top-nav scrollbar

| Field | Value |
|-------|-------|
| SHA | [`210c740`](https://github.com/Kn0w-1/AigentZBeta/commit/210c740bdb61c6e26b247bf90828b7395cdbf313) |
| Author | Claude |
| Date | 2026-05-26T10:50:08Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
4-tier nav row + Marketa Admin in metaMe + Studio Admin stub + top-nav scrollbar

Four fixes from the 2026-05-26 round of operator reports.

1) Tier-4 nav row (Order of Metayé > Admin in metaMe)
-----------------------------------------------------
The nav system rendered 3 tiers max. metaMe's mirror of KNYT's
Order group is a single-tab parent group — knytOrderTabs() output
gets promoted into the tier-2 slot. When the operator picked Admin
from that promoted row, Admin's own subTabs (cloned KNYT admin tabs)
sat at effective tier-4 with no render path → TabRendererFallback
showed "Select a sub-tab to continue" and the operator was stuck.

Adds a fourth tier:
- New state `activeSubSubSubTabSlug` and setter
- New memo `activeSubSubTabSubTabs` (filtered subTabs of the
  currently-selected tier-3 tab, with the same adminOnly +
  adminOfCartridge defense-in-depth gates that tier-3 applies)
- New memo `activeSubSubSubTab`
- Reset effect on tier-3 change → clears tier-4
- New nav row beneath the existing tier-3 row, fires whenever
  activeSubSubTabSubTabs.length > 0 (catches BOTH single-tab and
  multi-tab parent group cases)
- TabRenderer call changes to
  `activeSubSubSubTab ?? activeSubSubTab ?? activeTab` so the deepest
  selected leaf wins

2) Marketa Admin sub-tab in metaMe.marketa group
------------------------------------------------
metaMe's `marketa` tabGroup has hand-written tabs (not a pure mirror
helper), so the Admin sub-menu I added to MARKETA_CARTRIDGE in the
prior commit didn't flow through automatically. Declares the Admin
sub-tab explicitly in metaMe's marketa group with the same
adminOfCartridge: 'marketa' gate and a lazy getter that clones
MARKETA_CARTRIDGE admin tabGroup children. Now visible inside metaMe
just like the KNYT mirror.

3) metaMe Studio Admin stub
---------------------------
metaMe Studio is a single-page surface with no tier-2 sub-tabs. Per
operator: every activation group should consistently expose an Admin
entry for admins so the chief-of-staff protocol applies uniformly.
Stubs `studio-admin` via PlaceholderTab with adminOfCartridge:
'metame'. Real Studio admin tooling (template publishing controls,
bundle versioning, surface-plan review) populates this when it ships.

4) Top-nav carousel scrollbar sticking
--------------------------------------
metaMe Cartridge's primary tab bar uses overflow-x-auto for the
horizontal carousel, but unlike the tier-2/3/4 rows below it didn't
carry no-scrollbar. Firefox / WebKit render a persistent scrollbar
track on the container even when overflow isn't active, so the
operator saw a stub bar that never disappeared. Adds no-scrollbar
to the top container to match the inner rows.

Net visibility outcomes after deploy
------------------------------------
- KNYT cartridge Order > Admin → operates as before (tier-3 now)
- metaMe Order of Metayé > tier-2 promoted row > Admin > tier-3
  shows the cloned KNYT admin tabs (the previously-missing nav row)
- metaMe Marketa group > Admin sub-tab > tier-3 shows the cloned
  Marketa admin tabs
- metaMe Studio > Studio Admin sub-tab shows the PlaceholderTab stub
- Top nav bar no longer shows the persistent scrollbar stub
```

## Body

Four fixes from the 2026-05-26 round of operator reports.

1) Tier-4 nav row (Order of Metayé > Admin in metaMe)
-----------------------------------------------------
The nav system rendered 3 tiers max. metaMe's mirror of KNYT's
Order group is a single-tab parent group — knytOrderTabs() output
gets promoted into the tier-2 slot. When the operator picked Admin
from that promoted row, Admin's own subTabs (cloned KNYT admin tabs)
sat at effective tier-4 with no render path → TabRendererFallback
showed "Select a sub-tab to continue" and the operator was stuck.

Adds a fourth tier:
- New state `activeSubSubSubTabSlug` and setter
- New memo `activeSubSubTabSubTabs` (filtered subTabs of the
  currently-selected tier-3 tab, with the same adminOnly +
  adminOfCartridge defense-in-depth gates that tier-3 applies)
- New memo `activeSubSubSubTab`
- Reset effect on tier-3 change → clears tier-4
- New nav row beneath the existing tier-3 row, fires whenever
  activeSubSubTabSubTabs.length > 0 (catches BOTH single-tab and
  multi-tab parent group cases)
- TabRenderer call changes to
  `activeSubSubSubTab ?? activeSubSubTab ?? activeTab` so the deepest
  selected leaf wins

2) Marketa Admin sub-tab in metaMe.marketa group
------------------------------------------------
metaMe's `marketa` tabGroup has hand-written tabs (not a pure mirror
helper), so the Admin sub-menu I added to MARKETA_CARTRIDGE in the
prior commit didn't flow through automatically. Declares the Admin
sub-tab explicitly in metaMe's marketa group with the same
adminOfCartridge: 'marketa' gate and a lazy getter that clones
MARKETA_CARTRIDGE admin tabGroup children. Now visible inside metaMe
just like the KNYT mirror.

3) metaMe Studio Admin stub
---------------------------
metaMe Studio is a single-page surface with no tier-2 sub-tabs. Per
operator: every activation group should consistently expose an Admin
entry for admins so the chief-of-staff protocol applies uniformly.
Stubs `studio-admin` via PlaceholderTab with adminOfCartridge:
'metame'. Real Studio admin tooling (template publishing controls,
bundle versioning, surface-plan review) populates this when it ships.

4) Top-nav carousel scrollbar sticking
--------------------------------------
metaMe Cartridge's primary tab bar uses overflow-x-auto for the
horizontal carousel, but unlike the tier-2/3/4 rows below it didn't
carry no-scrollbar. Firefox / WebKit render a persistent scrollbar
track on the container even when overflow isn't active, so the
operator saw a stub bar that never disappeared. Adds no-scrollbar
to the top container to match the inner rows.

Net visibility outcomes after deploy
------------------------------------
- KNYT cartridge Order > Admin → operates as before (tier-3 now)
- metaMe Order of Metayé > tier-2 promoted row > Admin > tier-3
  shows the cloned KNYT admin tabs (the previously-missing nav row)
- metaMe Marketa group > Admin sub-tab > tier-3 shows the cloned
  Marketa admin tabs
- metaMe Studio > Studio Admin sub-tab shows the PlaceholderTab stub
- Top nav bar no longer shows the persistent scrollbar stub

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/CodexPanelDynamic.tsx` |
| Modified | `data/codex-configs.ts` |

## Stats

 2 files changed, 137 insertions(+), 2 deletions(-)
