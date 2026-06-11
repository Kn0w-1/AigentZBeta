# Commit Brief: `57cca6e` — migration: persona_iqube_holdings projection table

| Field | Value |
|-------|-------|
| SHA | [`57cca6e`](https://github.com/Kn0w-1/AigentZBeta/commit/57cca6e51f4647c77f838a088ea23d7722acba48) |
| Author | Claude |
| Date | 2026-05-26T17:43:13Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
migration: persona_iqube_holdings projection table

Per-persona projection of iQube ownership. References registry_assets
(the SoT per the iQube fleshing-out workstream); carries acquisition
delta only. Bespoke per-instance content stays in the iQube blakQube,
read via discloseCredential() on the spine.

Single table for all asset classes (mirrors persona_activations shape)
so the resolver doesn't fan out across five mirror tables. Empty until
the iQube workstream wires writes — the resolver returns
ownedAssets.iQubes = [] gracefully in the meantime.

Service-role only RLS. T0 persona_id never serialised; reads land via
/api/admin/* with spine isAdmin gating.

Operator action: apply in Supabase dev SQL editor before the
getPersonaAssetGraph resolver is exercised in dev.
```

## Body

Per-persona projection of iQube ownership. References registry_assets
(the SoT per the iQube fleshing-out workstream); carries acquisition
delta only. Bespoke per-instance content stays in the iQube blakQube,
read via discloseCredential() on the spine.

Single table for all asset classes (mirrors persona_activations shape)
so the resolver doesn't fan out across five mirror tables. Empty until
the iQube workstream wires writes — the resolver returns
ownedAssets.iQubes = [] gracefully in the meantime.

Service-role only RLS. T0 persona_id never serialised; reads land via
/api/admin/* with spine isAdmin gating.

Operator action: apply in Supabase dev SQL editor before the
getPersonaAssetGraph resolver is exercised in dev.

## Files Changed

| Change | File |
|--------|------|
| Added | `supabase/migrations/20260526010000_persona_iqube_holdings.sql` |

## Stats

 1 file changed, 116 insertions(+)
