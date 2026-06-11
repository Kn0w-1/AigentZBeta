# Commit Brief: `ef3d1ed` — metaMe / KNYT interface hygiene: 3 layout moves

| Field | Value |
|-------|-------|
| SHA | [`ef3d1ed`](https://github.com/Kn0w-1/AigentZBeta/commit/ef3d1edf5e45e4a37a0a7576669d779392eb37c8) |
| Author | Claude |
| Date | 2026-05-22T08:28:01Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
metaMe / KNYT interface hygiene: 3 layout moves

1) Move 'Community' tab from KNYT's top-level nav into its Order sub-group
   (group: 'order-group', order: 6). Because metaMe's Order of Metayé
   already mirrors knytOrderTabs() into its subTabs, this change
   automatically makes Community visible inside the metaMe Cartridge ->
   Order of Metayé surface too — no separate wiring needed.

2) Hoist the sub-sub-tabs row up onto the breadcrumb row for any
   single-tab group that has subTabs (notably Order of Metayé). When
   activeGroupSubTabs.length === 1 AND activeSubTabs.length > 0, render
   the sub-tab pills in the sub-header's left slot instead of in a
   separate row below. Removes the redundant second row and matches the
   aigentMe layout convention where sub-tabs and breadcrumb share a row.

3) Pin a 'Welcome, <persona>' badge to the cartridge header row,
   right-justified, immediately to the left of the theme toggle. Uses
   the existing activePersonaLabel from useCartridgePersonaGuard so it
   reflects the same persona the rest of the cartridge resolves. Sits
   above all tabs and stays visible across tab navigation. Hidden on
   small screens (md breakpoint) to avoid crowding the mobile header.
```

## Body

1) Move 'Community' tab from KNYT's top-level nav into its Order sub-group
   (group: 'order-group', order: 6). Because metaMe's Order of Metayé
   already mirrors knytOrderTabs() into its subTabs, this change
   automatically makes Community visible inside the metaMe Cartridge ->
   Order of Metayé surface too — no separate wiring needed.

2) Hoist the sub-sub-tabs row up onto the breadcrumb row for any
   single-tab group that has subTabs (notably Order of Metayé). When
   activeGroupSubTabs.length === 1 AND activeSubTabs.length > 0, render
   the sub-tab pills in the sub-header's left slot instead of in a
   separate row below. Removes the redundant second row and matches the
   aigentMe layout convention where sub-tabs and breadcrumb share a row.

3) Pin a 'Welcome, <persona>' badge to the cartridge header row,
   right-justified, immediately to the left of the theme toggle. Uses
   the existing activePersonaLabel from useCartridgePersonaGuard so it
   reflects the same persona the rest of the cartridge resolves. Sits
   above all tabs and stays visible across tab navigation. Hidden on
   small screens (md breakpoint) to avoid crowding the mobile header.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/CodexPanelDynamic.tsx` |
| Modified | `data/codex-configs.ts` |

## Stats

 2 files changed, 51 insertions(+), 29 deletions(-)
