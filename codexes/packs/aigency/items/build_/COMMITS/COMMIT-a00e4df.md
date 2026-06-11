# Commit Brief: `a00e4df` — share modal: neutral chrome for X + Email buttons (drop black/gray solid boxes)

| Field | Value |
|-------|-------|
| SHA | [`a00e4df`](https://github.com/Kn0w-1/AigentZBeta/commit/a00e4df300002a0fa84f10f49e38508e1a7ae564) |
| Author | Claude |
| Date | 2026-05-23T10:41:59Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
share modal: neutral chrome for X + Email buttons (drop black/gray solid boxes)

User feedback on the modal styling: X and Email stood out from the
rest of the network grid because they had solid black / gray
backgrounds while the other brand buttons used their official brand
colors. Switch both to the same neutral outlined chrome we use across
the rest of the modals (bg-white/5 hover:bg-white/10 border
border-white/10) so they read as 'generic icon' rather than 'visually
distinct'. The white icons stay; only the background plate changes.
```

## Body

User feedback on the modal styling: X and Email stood out from the
rest of the network grid because they had solid black / gray
backgrounds while the other brand buttons used their official brand
colors. Switch both to the same neutral outlined chrome we use across
the rest of the modals (bg-white/5 hover:bg-white/10 border
border-white/10) so they read as 'generic icon' rather than 'visually
distinct'. The white icons stay; only the background plate changes.

## Files Changed

| Change | File |
|--------|------|
| Modified | `packages/smarttriad/src/SocialSharingModal.tsx` |

## Stats

 1 file changed, 2 insertions(+), 2 deletions(-)
