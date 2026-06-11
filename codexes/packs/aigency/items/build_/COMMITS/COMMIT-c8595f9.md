# Commit Brief: `c8595f9` — move Quests to a sub-tab under Order group

| Field | Value |
|-------|-------|
| SHA | [`c8595f9`](https://github.com/Kn0w-1/AigentZBeta/commit/c8595f92826109e0db63e56b3c6f18a52ac50b61) |
| Author | Claude |
| Date | 2026-05-20T02:00:33Z |
| Branch | dev (direct push) |
| Type | `refactor` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
move Quests to a sub-tab under Order group

Quests is a sub-domain of Order — the canonical surface that explains
the four families of tasks that drive Order ascension. Previously
shipped as a top-level standalone tab between Order group and 21 Sats
(order: 3.5); now slotted as a sub-tab in the Order group between
Runtime (2) and KNYT Shelf (3) at order: 2.5.

The metaMe Cartridge's 'Order of Metayé' mirror (which pulls KNYT
order-group tabs via knytOrderTabs() in codex-configs) will pick up
Quests automatically — no metaMe-side change needed.
```

## Body

Quests is a sub-domain of Order — the canonical surface that explains
the four families of tasks that drive Order ascension. Previously
shipped as a top-level standalone tab between Order group and 21 Sats
(order: 3.5); now slotted as a sub-tab in the Order group between
Runtime (2) and KNYT Shelf (3) at order: 2.5.

The metaMe Cartridge's 'Order of Metayé' mirror (which pulls KNYT
order-group tabs via knytOrderTabs() in codex-configs) will pick up
Quests automatically — no metaMe-side change needed.

## Files Changed

| Change | File |
|--------|------|
| Modified | `data/codex-configs.ts` |

## Stats

 1 file changed, 3 insertions(+), 2 deletions(-)
