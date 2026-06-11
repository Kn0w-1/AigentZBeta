# Commit Brief: `c74f4db` — Q¢ payment fallback chain: wallet → DVN → buy, + drawer deep-link, + wallet recognition

| Field | Value |
|-------|-------|
| SHA | [`c74f4db`](https://github.com/Kn0w-1/AigentZBeta/commit/c74f4db28a7a2018fcc1e4298dae774f692133b4) |
| Author | Claude |
| Date | 2026-05-22T17:33:32Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Q¢ payment fallback chain: wallet → DVN → buy, + drawer deep-link, + wallet recognition

Three issues reported on the previous Q¢ build:

  1. User had 40 DVN Q¢ but was forced into the EVM external-wallet
     prompt with no way to choose DVN payment instead.
  2. The 'Connect an EVM wallet from the wallet drawer' banner had no
     deep-link — user had to find the drawer themselves.
  3. After connecting a wallet in the drawer and returning to the
     dialog, the dialog's useExternalWallet instance didn't recognise
     the connection.

Server side
  - qcPaymentIntent.ts — payment intent now carries dvnAvailable so the
    client can decide whether DVN-fallback is viable.
  - generate.ts::debitQc — accepts a paymentMode parameter:
      'auto' (default) → try DVN → custodial atomic → 402 with intent
      'dvn'            → skip atomic, debit DVN directly; if DVN can't
                         cover either, return 402 with needsBuyQc: true
  - generate/route.ts — reads paymentMode from request body, forwards
    needsBuyQc in the response when set.

Client side
  - RemixDialog.tsx
    • submit() now takes a paymentMode arg and posts it through.
    • New panel branches:
      - paymentIntent set → PaymentIntentPanel (revamped, see below)
      - needsBuyQc set    → BuyQcPanel (terminal 'top up Q¢' state)
    • Polls wallet.refresh() every 1s while the payment panel is open
      and no wallet is connected — picks up the session within ~1s of
      the user connecting in the drawer.
    • New onConnectWallet prop (deep-link target).

  - PaymentIntentPanel revamp
    • 'No EVM wallet connected' notice is now a clickable button
      labelled 'Open wallet drawer →' that fires onConnectWallet.
    • When intent.dvnAvailable >= amountQc, an emerald 'Pay X Q¢ from
      DVN' button sits beside the indigo 'Pay X Q¢ on Base Sepolia'
      button — user choice, not forced.
    • Clicking the DVN button calls submit('dvn') which routes the
      server through the DVN-only path.

  - BuyQcPanel (new) — terminal state of the fallback chain. Renders
    when /generate returns needsBuyQc. Amber styling, 'Buy more Q¢'
    button wired to onConnectWallet (opens drawer where the user can
    top up).

  - RuntimeCapsuleRemixEditor — forwards onConnectWallet through to
    RemixDialog.

  - MetaMeRuntimeClient — wires onConnectWallet to
    setWalletInitialTab('wallet'); setWalletDrawerOpen(true). Drawer
    contains ExternalWalletConnect; user lands on it on open.

Resulting UX
  • DVN sufficient            → silent debit, no panel (unchanged).
  • DVN short, custodial works → silent server-side settlement, no
                                 panel (unchanged).
  • DVN short, no custodial    → PaymentIntentPanel shows BOTH wallet
                                 and DVN options when DVN can cover;
                                 wallet-only when DVN can't cover.
  • User picks DVN, DVN short  → BuyQcPanel with 'Buy more Q¢' CTA.

Wallet refresh during the 1s poll has minimal cost — wallet.refresh
just reads sessionStorage and reconnects to a discovered provider if
the saved address matches.
```

## Body

Three issues reported on the previous Q¢ build:

  1. User had 40 DVN Q¢ but was forced into the EVM external-wallet
     prompt with no way to choose DVN payment instead.
  2. The 'Connect an EVM wallet from the wallet drawer' banner had no
     deep-link — user had to find the drawer themselves.
  3. After connecting a wallet in the drawer and returning to the
     dialog, the dialog's useExternalWallet instance didn't recognise
     the connection.

Server side
  - qcPaymentIntent.ts — payment intent now carries dvnAvailable so the
    client can decide whether DVN-fallback is viable.
  - generate.ts::debitQc — accepts a paymentMode parameter:
      'auto' (default) → try DVN → custodial atomic → 402 with intent
      'dvn'            → skip atomic, debit DVN directly; if DVN can't
                         cover either, return 402 with needsBuyQc: true
  - generate/route.ts — reads paymentMode from request body, forwards
    needsBuyQc in the response when set.

Client side
  - RemixDialog.tsx
    • submit() now takes a paymentMode arg and posts it through.
    • New panel branches:
      - paymentIntent set → PaymentIntentPanel (revamped, see below)
      - needsBuyQc set    → BuyQcPanel (terminal 'top up Q¢' state)
    • Polls wallet.refresh() every 1s while the payment panel is open
      and no wallet is connected — picks up the session within ~1s of
      the user connecting in the drawer.
    • New onConnectWallet prop (deep-link target).

  - PaymentIntentPanel revamp
    • 'No EVM wallet connected' notice is now a clickable button
      labelled 'Open wallet drawer →' that fires onConnectWallet.
    • When intent.dvnAvailable >= amountQc, an emerald 'Pay X Q¢ from
      DVN' button sits beside the indigo 'Pay X Q¢ on Base Sepolia'
      button — user choice, not forced.
    • Clicking the DVN button calls submit('dvn') which routes the
      server through the DVN-only path.

  - BuyQcPanel (new) — terminal state of the fallback chain. Renders
    when /generate returns needsBuyQc. Amber styling, 'Buy more Q¢'
    button wired to onConnectWallet (opens drawer where the user can
    top up).

  - RuntimeCapsuleRemixEditor — forwards onConnectWallet through to
    RemixDialog.

  - MetaMeRuntimeClient — wires onConnectWallet to
    setWalletInitialTab('wallet'); setWalletDrawerOpen(true). Drawer
    contains ExternalWalletConnect; user lands on it on open.

Resulting UX
  • DVN sufficient            → silent debit, no panel (unchanged).
  • DVN short, custodial works → silent server-side settlement, no
                                 panel (unchanged).
  • DVN short, no custodial    → PaymentIntentPanel shows BOTH wallet
                                 and DVN options when DVN can cover;
                                 wallet-only when DVN can't cover.
  • User picks DVN, DVN short  → BuyQcPanel with 'Buy more Q¢' CTA.

Wallet refresh during the 1s poll has minimal cost — wallet.refresh
just reads sessionStorage and reconnects to a discovered provider if
the saved address matches.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/_lib/generate.ts` |
| Modified | `app/api/community-content/_lib/qcPaymentIntent.ts` |
| Modified | `app/api/community-content/generate/route.ts` |
| Modified | `components/metame/MetaMeRuntimeClient.tsx` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |
| Modified | `components/metame/runtime/RuntimeCapsuleRemixEditor.tsx` |

## Stats

 6 files changed, 236 insertions(+), 37 deletions(-)
