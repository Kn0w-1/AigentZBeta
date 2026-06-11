# Commit Brief: `6f99ad1` — persona asset graph: resolver + Persona 360 tab + lazy enrichment

| Field | Value |
|-------|-------|
| SHA | [`6f99ad1`](https://github.com/Kn0w-1/AigentZBeta/commit/6f99ad1b39ea19bca1f08268f48b057dddf4a4a9) |
| Author | Claude |
| Date | 2026-05-26T17:49:17Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
persona asset graph: resolver + Persona 360 tab + lazy enrichment

Builds the read-side projection the CRM enrichment plan proposed,
now that the operator has confirmed iQube Registry as SoT.

Resolver
- services/identity/personaAssetGraph.ts — getPersonaAssetGraph(personaId)
  composes the existing canonical resolvers (cartridgeAdminGrants,
  userOwnsAsset/getOwnedAssetIds, persona_activations,
  wallet_alias_commitments, crm_personas, crm_investors,
  knyt_reward_grants, registry_assets via persona_iqube_holdings,
  marketa_agent_personas). No parallel sources of truth — every read
  goes through the existing helper. Branches resolved in parallel;
  per-branch failures swallow so the response always returns a
  usable shape. T1-safe payload only — wallet linkage exposed as
  (chain, status, expiresAt) triples, never plaintext addresses.
  iQube branch reads persona_iqube_holdings (empty until the iQube
  workstream wires writes; returns []).

API
- GET /api/admin/access-requests/[id]/graph — lazy-loaded graph for
  the reviewer, on row expand. Resolves the requester personaId from
  the request row (never trusts a client-supplied id).
- GET /api/admin/persona-graph?personaId=... — direct graph fetch
  for the Persona 360 tab.
- GET /api/admin/persona-graph/search?q=... — picker lookup against
  display label, FIO handle, crm email, persona id.

UI
- components/metame/admin/PersonaAssetGraphView.tsx — shared T1
  renderer (six panels: identity, wallet aliases, reputation,
  admin scopes, activations, commercial, owned content, owned
  iQubes, agents). Stateless; both surfaces use it.
- AdminAccessRequestsTab: row expand now lazy-fetches the graph
  and renders the shared view beneath the alpha enrichment grid.
  Cached per row so re-expand doesn't re-fetch.
- Persona360InspectorTab (new) — debounced search + persona picker
  + graph render. Sits under metame-codex/admin/persona-360.

Privacy posture: alpha PII rule is in effect (full email + FIO
handle visible to platform admins). The consent surface backlog
(2026-05-26_pii-exposure-consent-surface-backlog.md) governs the
follow-on mask-by-audience layer.
```

## Body

Builds the read-side projection the CRM enrichment plan proposed,
now that the operator has confirmed iQube Registry as SoT.

Resolver
- services/identity/personaAssetGraph.ts — getPersonaAssetGraph(personaId)
  composes the existing canonical resolvers (cartridgeAdminGrants,
  userOwnsAsset/getOwnedAssetIds, persona_activations,
  wallet_alias_commitments, crm_personas, crm_investors,
  knyt_reward_grants, registry_assets via persona_iqube_holdings,
  marketa_agent_personas). No parallel sources of truth — every read
  goes through the existing helper. Branches resolved in parallel;
  per-branch failures swallow so the response always returns a
  usable shape. T1-safe payload only — wallet linkage exposed as
  (chain, status, expiresAt) triples, never plaintext addresses.
  iQube branch reads persona_iqube_holdings (empty until the iQube
  workstream wires writes; returns []).

API
- GET /api/admin/access-requests/[id]/graph — lazy-loaded graph for
  the reviewer, on row expand. Resolves the requester personaId from
  the request row (never trusts a client-supplied id).
- GET /api/admin/persona-graph?personaId=... — direct graph fetch
  for the Persona 360 tab.
- GET /api/admin/persona-graph/search?q=... — picker lookup against
  display label, FIO handle, crm email, persona id.

UI
- components/metame/admin/PersonaAssetGraphView.tsx — shared T1
  renderer (six panels: identity, wallet aliases, reputation,
  admin scopes, activations, commercial, owned content, owned
  iQubes, agents). Stateless; both surfaces use it.
- AdminAccessRequestsTab: row expand now lazy-fetches the graph
  and renders the shared view beneath the alpha enrichment grid.
  Cached per row so re-expand doesn't re-fetch.
- Persona360InspectorTab (new) — debounced search + persona picker
  + graph render. Sits under metame-codex/admin/persona-360.

Privacy posture: alpha PII rule is in effect (full email + FIO
handle visible to platform admins). The consent surface backlog
(2026-05-26_pii-exposure-consent-surface-backlog.md) governs the
follow-on mask-by-audience layer.

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/admin/access-requests/[id]/graph/route.ts` |
| Added | `app/api/admin/persona-graph/route.ts` |
| Added | `app/api/admin/persona-graph/search/route.ts` |
| Modified | `app/triad/components/codex/TabRenderer.tsx` |
| Modified | `app/triad/components/codex/tabs/AdminAccessRequestsTab.tsx` |
| Added | `app/triad/components/codex/tabs/Persona360InspectorTab.tsx` |
| Added | `components/metame/admin/PersonaAssetGraphView.tsx` |
| Modified | `data/codex-configs.ts` |
| Added | `services/identity/personaAssetGraph.ts` |

## Stats

 9 files changed, 1248 insertions(+), 1 deletion(-)
