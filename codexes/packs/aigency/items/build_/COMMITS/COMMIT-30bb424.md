# Commit Brief: `30bb424` — plan: CRM <-> identity / asset enrichment followup doc

| Field | Value |
|-------|-------|
| SHA | [`30bb424`](https://github.com/Kn0w-1/AigentZBeta/commit/30bb424f623c79efee66eea9f5d9083920303264) |
| Author | Claude |
| Date | 2026-05-26T16:57:30Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
plan: CRM <-> identity / asset enrichment followup doc

Captures the broader CRM-identity-data-model enrichment the operator
asked about in the refinements pass (linking personas, DIDQube
identities, SmartWallet alias commitments, AIQS, agents, AigentQubes,
etc. so admin reviewers and aigentMe have a full read of the
requester's identity-and-asset graph). Not implemented; this is a
grounded planning doc that builds on what the platform already has:

- The CRM <-> persona link already exists via crm_personas + the
  smart-link trigger + the crm_personas_with_identity view.
- DIDQube fields (root_did, kybe_did, fio_handle) already wired.
- Wallet linkage already privacy-preserving via
  wallet_alias_commitments (commitment-only; plaintext lives in
  blakQube and never in queryable tables).
- Admin grants resolved through getActivePersona today.

The genuine gap is the persona-grain ownership projection for iQube
assets (AigentQube, TalentQube, DataQube, etc.) and a unified
read-side resolver. Doc proposes one new table
(persona_iqube_holdings) + one new resolver (getPersonaAssetGraph)
that composes existing spine resolvers without forking them.

Registered in agentiq/collections.json under col_updates so the
Updates tab surfaces it.
```

## Body

Captures the broader CRM-identity-data-model enrichment the operator
asked about in the refinements pass (linking personas, DIDQube
identities, SmartWallet alias commitments, AIQS, agents, AigentQubes,
etc. so admin reviewers and aigentMe have a full read of the
requester's identity-and-asset graph). Not implemented; this is a
grounded planning doc that builds on what the platform already has:

- The CRM <-> persona link already exists via crm_personas + the
  smart-link trigger + the crm_personas_with_identity view.
- DIDQube fields (root_did, kybe_did, fio_handle) already wired.
- Wallet linkage already privacy-preserving via
  wallet_alias_commitments (commitment-only; plaintext lives in
  blakQube and never in queryable tables).
- Admin grants resolved through getActivePersona today.

The genuine gap is the persona-grain ownership projection for iQube
assets (AigentQube, TalentQube, DataQube, etc.) and a unified
read-side resolver. Doc proposes one new table
(persona_iqube_holdings) + one new resolver (getPersonaAssetGraph)
that composes existing spine resolvers without forking them.

Registered in agentiq/collections.json under col_updates so the
Updates tab surfaces it.

## Files Changed

| Change | File |
|--------|------|
| Modified | `codexes/packs/agentiq/collections.json` |
| Added | `codexes/packs/agentiq/updates/2026-05-26_crm-identity-asset-enrichment-plan.md` |

## Stats

 2 files changed, 219 insertions(+)
