# Commit Brief: `7ec1f50` — Q¢ atomic/canonical path via custodial agent_keys (server-signs, no user prompt)

| Field | Value |
|-------|-------|
| SHA | [`7ec1f50`](https://github.com/Kn0w-1/AigentZBeta/commit/7ec1f50ff2f83415f0fd6051f9dc69ccb48afd9d) |
| Author | Claude |
| Date | 2026-05-22T16:33:35Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Q¢ atomic/canonical path via custodial agent_keys (server-signs, no user prompt)

The user's question — 'why don't SmartWallets work like A2A?' — was
the right one. The answer was an architectural layer I missed:
agent_keys table holds encrypted private keys for ALL entities that
the platform can transact on behalf of, including FIO-handle personas
(via the persona_id column). The A2A signer endpoint already uses
this; my x402 build was forcing MetaMask signatures for users whose
keys the platform already custodies.

This commit makes Q¢ debit attempt the canonical/atomic path FIRST:

  1) DVN check — if balance covers, debit normally (existing).
  2) DVN short — call attemptCustodialSettlement() which POSTs to
     /api/a2a/signer/transfer with { agentId: personaId, chainId,
     tokenAddress, to: TREASURY, amount }. The A2A endpoint looks up
     agent_keys by persona_id, decrypts the PK, server-signs the
     QCT.transfer to MoneyPenny treasury, and triggers the
     existing DVN/PoS receipt flow.
  3) On settlement success — credit DVN with the settled amount
     (DVN-in-the-loop per the parity model), refetch balance, fall
     through to the standard debit loop. ZERO user signature, zero
     wallet prompt, identical UX to A2A.
  4) Only when the persona has no agent_keys row (404 from signer)
     or custodial wallet itself is empty (400 'Insufficient') —
     fall back to the x402 + external-wallet path shipped earlier
     today, where the user signs via MetaMask et al.
  5) Genuine signer errors (5xx) surface as 500 to the client.

The settlement records two audit rows linked by reference_id:
  - qc_transactions { type:'custodial_settlement', tx_id: 'settle::onchain' }
  - qc_transactions { type:'debit', tx_id: 'dvn-qc-...' }

Files:
  + app/api/community-content/_lib/custodialSettlement.ts (new helper)
  ~ app/api/community-content/_lib/generate.ts::debitQc (canonical-first)

Backlog brief landed alongside this commit:
  codexes/packs/agentiq/updates/
    2026-05-22_qc-payment-modes-control-framework-backlog.md

Captures the three-mode framework (atomic / deferred / remote), the
two layers of admin control (Studio super-admin allowed-set + admin
customizer per-deployment radio group), tooltip copy, schema
additions, and phasing. Atomic = shipped today; B = wallet parity
sweep; C = admin radios; D = Studio gates; E = deferred + batch;
F = remote-custody mode.
```

## Body

The user's question — 'why don't SmartWallets work like A2A?' — was
the right one. The answer was an architectural layer I missed:
agent_keys table holds encrypted private keys for ALL entities that
the platform can transact on behalf of, including FIO-handle personas
(via the persona_id column). The A2A signer endpoint already uses
this; my x402 build was forcing MetaMask signatures for users whose
keys the platform already custodies.

This commit makes Q¢ debit attempt the canonical/atomic path FIRST:

  1) DVN check — if balance covers, debit normally (existing).
  2) DVN short — call attemptCustodialSettlement() which POSTs to
     /api/a2a/signer/transfer with { agentId: personaId, chainId,
     tokenAddress, to: TREASURY, amount }. The A2A endpoint looks up
     agent_keys by persona_id, decrypts the PK, server-signs the
     QCT.transfer to MoneyPenny treasury, and triggers the
     existing DVN/PoS receipt flow.
  3) On settlement success — credit DVN with the settled amount
     (DVN-in-the-loop per the parity model), refetch balance, fall
     through to the standard debit loop. ZERO user signature, zero
     wallet prompt, identical UX to A2A.
  4) Only when the persona has no agent_keys row (404 from signer)
     or custodial wallet itself is empty (400 'Insufficient') —
     fall back to the x402 + external-wallet path shipped earlier
     today, where the user signs via MetaMask et al.
  5) Genuine signer errors (5xx) surface as 500 to the client.

The settlement records two audit rows linked by reference_id:
  - qc_transactions { type:'custodial_settlement', tx_id: 'settle::onchain' }
  - qc_transactions { type:'debit', tx_id: 'dvn-qc-...' }

Files:
  + app/api/community-content/_lib/custodialSettlement.ts (new helper)
  ~ app/api/community-content/_lib/generate.ts::debitQc (canonical-first)

Backlog brief landed alongside this commit:
  codexes/packs/agentiq/updates/
    2026-05-22_qc-payment-modes-control-framework-backlog.md

Captures the three-mode framework (atomic / deferred / remote), the
two layers of admin control (Studio super-admin allowed-set + admin
customizer per-deployment radio group), tooltip copy, schema
additions, and phasing. Atomic = shipped today; B = wallet parity
sweep; C = admin radios; D = Studio gates; E = deferred + batch;
F = remote-custody mode.

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/community-content/_lib/custodialSettlement.ts` |
| Modified | `app/api/community-content/_lib/generate.ts` |
| Modified | `codexes/packs/agentiq/collections.json` |
| Added | `codexes/packs/agentiq/updates/2026-05-22_qc-payment-modes-control-framework-backlog.md` |

## Stats

 4 files changed, 394 insertions(+), 23 deletions(-)
