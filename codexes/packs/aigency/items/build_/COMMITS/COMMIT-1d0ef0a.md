# Commit Brief: `1d0ef0a` — Q¢ x402 on-chain settlement via Base Sepolia (mainnet-switchable)

| Field | Value |
|-------|-------|
| SHA | [`1d0ef0a`](https://github.com/Kn0w-1/AigentZBeta/commit/1d0ef0a27bc2c8a81db758d67a3526ffc8667321) |
| Author | Claude |
| Date | 2026-05-22T15:41:26Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Q¢ x402 on-chain settlement via Base Sepolia (mainnet-switchable)

When a persona's DVN Q¢ balance can't cover a community-content remix,
the user can now settle the difference on-chain by signing a QCT
transfer to the MoneyPenny treasury. Reuses the existing facilitator
verify pipe (/api/a2a/facilitator/verify) and existing external-wallet
discovery (useExternalWallet, EIP-6963) so there's no new RPC or
signer infrastructure.

Server side
  - app/api/community-content/_lib/qcPaymentIntent.ts (NEW)
    Builds the x402-style envelope { intentId, asset:'QCT', chainId,
    tokenAddress, payTo, amount, amountQc, currency, deadline } and
    persists a 'settlement_pending' row in qc_transactions so the
    settle endpoint can verify against a server-trusted amount, not
    client-supplied values. Env vars QCT_TOKEN_ADDRESS, QCT_CHAIN_ID,
    TREASURY_ADDRESS govern the chain/token/treasury — flip them to
    swap testnet → mainnet Base without code changes.

  - app/api/community-content/_lib/generate.ts::debitQc
    On insufficient DVN, calls createQcPaymentIntent and returns the
    envelope alongside the existing 402 error. Callers can keep
    treating the error as 'insufficient' OR surface the payment field
    to trigger a wallet flow.

  - app/api/community-content/generate/route.ts
    Forwards debit.payment in the 402 response body so RemixDialog
    can read it.

  - app/api/community-content/settle/route.ts (NEW)
    POST { intentId, txHash }. Auth via getActivePersona, refuses
    cross-persona settlement. Looks up the pending intent, computes
    expected amount in 18-decimal base units, calls the facilitator
    verify endpoint internally to confirm the on-chain ERC20 Transfer
    log matches { tokenAddress, payTo, amount }, credits DVN via
    creditQc, marks the intent settled to prevent replay.

Client side
  - components/metame/runtime/RemixDialog.tsx
    submit() now detects res.status === 402 with a 'payment' field and
    populates paymentIntent state instead of surfacing the raw
    'Insufficient Q¢' error. PaymentIntentPanel renders an inline
    'Pay X Q¢ on Base Sepolia' CTA showing the treasury + token
    address. payWithWallet() switches chain if needed, encodes
    ERC20 transfer(address,uint256) calldata inline (no ABI lib), calls
    eth_sendTransaction via the user's connected provider, POSTs the
    txHash to /settle, and re-runs submit() on success so DVN debit
    proceeds normally.

Refund path on discard remains DVN-only (creditQc) per the user's
direction — value stays with the user, custody just shifts back to
the off-chain ledger.

Follow-ups deliberately not in this commit:
  - batch purchase of multiple remixes in one tx
  - DVN ⇄ on-chain 1:1 reconciliation job
  - deferred minting
  - production RPC overrides (Alchemy/Infura) — NEXT_PUBLIC_RPC_BASE_SEPAIA
    is already the wired env var; sepolia.base.org is the fallback.
```

## Body

When a persona's DVN Q¢ balance can't cover a community-content remix,
the user can now settle the difference on-chain by signing a QCT
transfer to the MoneyPenny treasury. Reuses the existing facilitator
verify pipe (/api/a2a/facilitator/verify) and existing external-wallet
discovery (useExternalWallet, EIP-6963) so there's no new RPC or
signer infrastructure.

Server side
  - app/api/community-content/_lib/qcPaymentIntent.ts (NEW)
    Builds the x402-style envelope { intentId, asset:'QCT', chainId,
    tokenAddress, payTo, amount, amountQc, currency, deadline } and
    persists a 'settlement_pending' row in qc_transactions so the
    settle endpoint can verify against a server-trusted amount, not
    client-supplied values. Env vars QCT_TOKEN_ADDRESS, QCT_CHAIN_ID,
    TREASURY_ADDRESS govern the chain/token/treasury — flip them to
    swap testnet → mainnet Base without code changes.

  - app/api/community-content/_lib/generate.ts::debitQc
    On insufficient DVN, calls createQcPaymentIntent and returns the
    envelope alongside the existing 402 error. Callers can keep
    treating the error as 'insufficient' OR surface the payment field
    to trigger a wallet flow.

  - app/api/community-content/generate/route.ts
    Forwards debit.payment in the 402 response body so RemixDialog
    can read it.

  - app/api/community-content/settle/route.ts (NEW)
    POST { intentId, txHash }. Auth via getActivePersona, refuses
    cross-persona settlement. Looks up the pending intent, computes
    expected amount in 18-decimal base units, calls the facilitator
    verify endpoint internally to confirm the on-chain ERC20 Transfer
    log matches { tokenAddress, payTo, amount }, credits DVN via
    creditQc, marks the intent settled to prevent replay.

Client side
  - components/metame/runtime/RemixDialog.tsx
    submit() now detects res.status === 402 with a 'payment' field and
    populates paymentIntent state instead of surfacing the raw
    'Insufficient Q¢' error. PaymentIntentPanel renders an inline
    'Pay X Q¢ on Base Sepolia' CTA showing the treasury + token
    address. payWithWallet() switches chain if needed, encodes
    ERC20 transfer(address,uint256) calldata inline (no ABI lib), calls
    eth_sendTransaction via the user's connected provider, POSTs the
    txHash to /settle, and re-runs submit() on success so DVN debit
    proceeds normally.

Refund path on discard remains DVN-only (creditQc) per the user's
direction — value stays with the user, custody just shifts back to
the off-chain ledger.

Follow-ups deliberately not in this commit:
  - batch purchase of multiple remixes in one tx
  - DVN ⇄ on-chain 1:1 reconciliation job
  - deferred minting
  - production RPC overrides (Alchemy/Infura) — NEXT_PUBLIC_RPC_BASE_SEPAIA
    is already the wired env var; sepolia.base.org is the fallback.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/_lib/generate.ts` |
| Added | `app/api/community-content/_lib/qcPaymentIntent.ts` |
| Modified | `app/api/community-content/generate/route.ts` |
| Added | `app/api/community-content/settle/route.ts` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 5 files changed, 558 insertions(+), 3 deletions(-)
