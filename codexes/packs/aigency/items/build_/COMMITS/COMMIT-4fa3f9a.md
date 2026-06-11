# Commit Brief: `4fa3f9a` — wallet send: restore network selector + resolve all recipients before transfer

| Field | Value |
|-------|-------|
| SHA | [`4fa3f9a`](https://github.com/Kn0w-1/AigentZBeta/commit/4fa3f9a488d185561fcd5796882ea6f5b1060047) |
| Author | Claude |
| Date | 2026-05-22T20:17:43Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
wallet send: restore network selector + resolve all recipients before transfer

Two bugs reported on the SmartWallet send modal:

  1. No way to pick the network — modal silently defaulted to Arbitrum
     Sepolia for every send. A2A wallet's assetKey dropdown was not
     mirrored here.
  2. Sending to a persona name like 'devagent' returned
     'network does not support ENS (chainId 421614)' — ethers.js tried
     to ENS-resolve the recipient because the modal passed the raw
     string through to erc20.transfer().

Fixes:

  • Un-comment the chain-selector grid in TransactionModal (was hidden
    behind a JSX comment at line 1010). Now mirrors A2ATestCard's
    assetKey dropdown: KNYT / Q¢ / USDC token selector PLUS a network
    selector showing every active EVM chain. Switch-wallet warning
    still surfaces when MetaMask is on a different chain than picked.

  • Route ALL recipients through /api/identity/resolve-recipient, not
    just @-handles / .fio handles. Resolver handles:
       0x EVM         pass-through
       @handle        nakamoto_knyt/qripto tables (existing)
       bare 'name'    agent_keys (NEW — covers persona handles and
                      anything the platform custodies a key for)
       did:iq:<hex>   reinsert hyphens → persona_id → agent_keys (NEW)
       name@domain    FIO lookup (existing)
    If the resolver returns no 0x address, the modal aborts with a
    clear error instead of calling erc20.transfer(<bare-string>, …)
    which is what triggered the ENS explosion on Arbitrum Sepolia.

  • Defensive final check — after resolution, the recipient must
    match /^0x[0-9a-fA-F]{40}$/ or we refuse to call the signer.
```

## Body

Two bugs reported on the SmartWallet send modal:

  1. No way to pick the network — modal silently defaulted to Arbitrum
     Sepolia for every send. A2A wallet's assetKey dropdown was not
     mirrored here.
  2. Sending to a persona name like 'devagent' returned
     'network does not support ENS (chainId 421614)' — ethers.js tried
     to ENS-resolve the recipient because the modal passed the raw
     string through to erc20.transfer().

Fixes:

  • Un-comment the chain-selector grid in TransactionModal (was hidden
    behind a JSX comment at line 1010). Now mirrors A2ATestCard's
    assetKey dropdown: KNYT / Q¢ / USDC token selector PLUS a network
    selector showing every active EVM chain. Switch-wallet warning
    still surfaces when MetaMask is on a different chain than picked.

  • Route ALL recipients through /api/identity/resolve-recipient, not
    just @-handles / .fio handles. Resolver handles:
       0x EVM         pass-through
       @handle        nakamoto_knyt/qripto tables (existing)
       bare 'name'    agent_keys (NEW — covers persona handles and
                      anything the platform custodies a key for)
       did:iq:<hex>   reinsert hyphens → persona_id → agent_keys (NEW)
       name@domain    FIO lookup (existing)
    If the resolver returns no 0x address, the modal aborts with a
    clear error instead of calling erc20.transfer(<bare-string>, …)
    which is what triggered the ENS explosion on Arbitrum Sepolia.

  • Defensive final check — after resolution, the recipient must
    match /^0x[0-9a-fA-F]{40}$/ or we refuse to call the signer.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/identity/resolve-recipient/route.ts` |
| Modified | `app/components/wallet/TransactionModal.tsx` |

## Stats

 2 files changed, 94 insertions(+), 13 deletions(-)
