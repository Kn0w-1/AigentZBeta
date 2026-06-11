# Commit Brief: `adecc28` — fix: registry ownership feeds canPurchase + buy-action ownership guard

| Field | Value |
|-------|-------|
| SHA | [`adecc28`](https://github.com/Kn0w-1/AigentZBeta/commit/adecc28453e8922add731fb6cecbe4fc015130dd) |
| Author | Claude |
| Date | 2026-05-15T22:39:58Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: registry ownership feeds canPurchase + buy-action ownership guard

Root cause of paywall-on-owned-content (KnytTab grid + scrolls + characters):

1. ContentCard.canPurchase = !item.metadata?.owned, but item.metadata.owned
   was only populated from ownedEpisodeNumbers (i.e. /api/codex/owned).
   When the legacy endpoint missed (FIO resolution failure, timing, etc.)
   but the ContentQube registry reported ownership, the Buy button still
   rendered on owned cards. Clicking it called handleSmartAction('buy')
   which calls openPurchaseForItem WITHOUT consulting isEpisodeLocked.

   Fix: contentWithOwnership now merges registryOwnership +
   ownedEpisodeNumbers when stamping item.metadata.owned. Either signal
   suppresses canPurchase.

2. handleSmartAction('buy') had no ownership check — it would open the
   purchase modal even on owned content if accidentally triggered.

   Fix: gate the buy branch on isEpisodeLocked. When owned, fall through
   to open the PDF viewer instead of the purchase modal.

3. The codex-grid card body onClick (line 3183) defers to cardAct.
   showShoppingCart. If cardAct's internal resolver (useOwnedAssets)
   hadn't populated yet but our local isOwned is true (registry OR
   legacy OR cached), the cart fired anyway.

   Fix: skip the cart branch when our local isOwned is true.

Also added [KnytTab:CARDCLICK] / [KnytTab:OPEN_PURCHASE_EP] /
[KnytTab:OPEN_PURCHASE_ITEM] diagnostic logging with stack traces so
any remaining failure can be traced to its exact entry point.

trigger deploy to dev
```

## Body

Root cause of paywall-on-owned-content (KnytTab grid + scrolls + characters):

1. ContentCard.canPurchase = !item.metadata?.owned, but item.metadata.owned
   was only populated from ownedEpisodeNumbers (i.e. /api/codex/owned).
   When the legacy endpoint missed (FIO resolution failure, timing, etc.)
   but the ContentQube registry reported ownership, the Buy button still
   rendered on owned cards. Clicking it called handleSmartAction('buy')
   which calls openPurchaseForItem WITHOUT consulting isEpisodeLocked.

   Fix: contentWithOwnership now merges registryOwnership +
   ownedEpisodeNumbers when stamping item.metadata.owned. Either signal
   suppresses canPurchase.

2. handleSmartAction('buy') had no ownership check — it would open the
   purchase modal even on owned content if accidentally triggered.

   Fix: gate the buy branch on isEpisodeLocked. When owned, fall through
   to open the PDF viewer instead of the purchase modal.

3. The codex-grid card body onClick (line 3183) defers to cardAct.
   showShoppingCart. If cardAct's internal resolver (useOwnedAssets)
   hadn't populated yet but our local isOwned is true (registry OR
   legacy OR cached), the cart fired anyway.

   Fix: skip the cart branch when our local isOwned is true.

Also added [KnytTab:CARDCLICK] / [KnytTab:OPEN_PURCHASE_EP] /
[KnytTab:OPEN_PURCHASE_ITEM] diagnostic logging with stack traces so
any remaining failure can be traced to its exact entry point.

trigger deploy to dev

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 82 insertions(+), 11 deletions(-)
