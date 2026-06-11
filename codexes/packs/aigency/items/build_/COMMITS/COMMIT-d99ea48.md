# Commit Brief: `d99ea48` — A2A wallet: accept FIO handles (any domain), DIDs, persona UUIDs as recipients

| Field | Value |
|-------|-------|
| SHA | [`d99ea48`](https://github.com/Kn0w-1/AigentZBeta/commit/d99ea48da567056b00bf98d1b0a8941ef574e1cc) |
| Author | Claude |
| Date | 2026-05-22T20:26:44Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
A2A wallet: accept FIO handles (any domain), DIDs, persona UUIDs as recipients

The A2A wallet's resolveRecipientAddress() only handled two cases:
  • @aigent-<name> / aigent-<name>  (local agent registry)
  • 0x… EVM address                 (pass-through)

Everything else hit 'Invalid recipient. Use agent ID (@aigent-name) or
Ethereum address (0x...)' — including legitimate FIO handles like
name@knyt or name@qripto, did:iq:<hex> DIDs, and bare persona UUIDs.
That blocks the agent → user payment direction entirely (agents can
pay other agents, but couldn't pay a human persona by their FIO
handle).

Rewrite as async and route every non-trivial input through
/api/identity/resolve-recipient — the same canonical resolver the
SmartWallet's TransactionModal now uses. The resolver queries
nakamoto_knyt_personas, nakamoto_qripto_personas, agent_keys (any
domain — knyt, qripto, aigent, etc), and the FIO service in turn.

Local agent-registry fast path is kept ahead of the round trip for the
@aigent-* case since it's by far the most common A2A pattern and
short-circuits the network call. If the local lookup misses we fall
through to the canonical resolver rather than throwing — covers
aigents in agent_keys that aren't in the local registry.

Updated the input placeholder to advertise the supported formats:
'@agent · name@fio-domain · did:iq:… · 0x…'

The error path now reads:
  Couldn't resolve "<input>" — use 0x address, @agent handle,
  name@fio-domain, or did:iq:<id>.
```

## Body

The A2A wallet's resolveRecipientAddress() only handled two cases:
  • @aigent-<name> / aigent-<name>  (local agent registry)
  • 0x… EVM address                 (pass-through)

Everything else hit 'Invalid recipient. Use agent ID (@aigent-name) or
Ethereum address (0x...)' — including legitimate FIO handles like
name@knyt or name@qripto, did:iq:<hex> DIDs, and bare persona UUIDs.
That blocks the agent → user payment direction entirely (agents can
pay other agents, but couldn't pay a human persona by their FIO
handle).

Rewrite as async and route every non-trivial input through
/api/identity/resolve-recipient — the same canonical resolver the
SmartWallet's TransactionModal now uses. The resolver queries
nakamoto_knyt_personas, nakamoto_qripto_personas, agent_keys (any
domain — knyt, qripto, aigent, etc), and the FIO service in turn.

Local agent-registry fast path is kept ahead of the round trip for the
@aigent-* case since it's by far the most common A2A pattern and
short-circuits the network call. If the local lookup misses we fall
through to the canonical resolver rather than throwing — covers
aigents in agent_keys that aren't in the local registry.

Updated the input placeholder to advertise the supported formats:
'@agent · name@fio-domain · did:iq:… · 0x…'

The error path now reads:
  Couldn't resolve "<input>" — use 0x address, @agent handle,
  name@fio-domain, or did:iq:<id>.

## Files Changed

| Change | File |
|--------|------|
| Modified | `components/AgentWalletDrawer.tsx` |

## Stats

 1 file changed, 52 insertions(+), 16 deletions(-)
