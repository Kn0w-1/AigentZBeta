# Commit Brief: `ac5d0f3` — revert: KnytTab gnEp capture back to episodeNumber === 0

| Field | Value |
|-------|-------|
| SHA | [`ac5d0f3`](https://github.com/Kn0w-1/AigentZBeta/commit/ac5d0f3b43b94cc201c6ba2ab65c96145c5adb3f) |
| Author | Claude |
| Date | 2026-05-16T03:07:56Z |
| Branch | dev (direct push) |
| Type | `revert` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
revert: KnytTab gnEp capture back to episodeNumber === 0

Reverts the second half of eced916d. The change to capture gnEp at -1
made DB ep 0 fall through the loop and produce an extra 'Gen Zero
Divided by One' card in the grid alongside the existing AGN preorder
tiles. Restore the legacy capture so DB ep 0 stays absorbed into the
AGN slot. The PDF loader timeout bump from eced916d is unchanged.

Off-by-one CID issue still needs to be addressed — kept separate per
operator direction.
```

## Body

Reverts the second half of eced916d. The change to capture gnEp at -1
made DB ep 0 fall through the loop and produce an extra 'Gen Zero
Divided by One' card in the grid alongside the existing AGN preorder
tiles. Restore the legacy capture so DB ep 0 stays absorbed into the
AGN slot. The PDF loader timeout bump from eced916d is unchanged.

Off-by-one CID issue still needs to be addressed — kept separate per
operator direction.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 1 file changed, 5 insertions(+), 11 deletions(-)
