# Commit Brief: `bb67913` — feat(content-qube): Phase 2 schema + types — 8 net-new ContentQube tables

| Field | Value |
|-------|-------|
| SHA | [`bb67913`](https://github.com/Kn0w-1/AigentZBeta/commit/bb67913b00be0a0e924f3a0b467b8f621fb9b46a) |
| Author | Claude |
| Date | 2026-05-13T18:46:36Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
feat(content-qube): Phase 2 schema + types — 8 net-new ContentQube tables

Adds the iQube Protocol content registry foundation:
- supabase/migrations/20260513010000_content_qubes_schema.sql
  content_qubes, content_qube_storage, content_qube_access_policies,
  content_qube_relationships, content_qube_cartridge_bindings,
  content_qube_editions (1,860-edition rarity ledger),
  content_qube_versions, content_qube_dvn_receipts
- types/contentQube.ts — full TypeScript contract; DB row types,
  browser-safe DisplayManifest (no T0 fields), service input types

Privacy: persona_id / author_persona_id marked T0 in schema + types.
Only t2_alias_commitment (T2) appears in dvn_receipts.
All tables RLS-gated (service_role only) per spine rules.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

Adds the iQube Protocol content registry foundation:
- supabase/migrations/20260513010000_content_qubes_schema.sql
  content_qubes, content_qube_storage, content_qube_access_policies,
  content_qube_relationships, content_qube_cartridge_bindings,
  content_qube_editions (1,860-edition rarity ledger),
  content_qube_versions, content_qube_dvn_receipts
- types/contentQube.ts — full TypeScript contract; DB row types,
  browser-safe DisplayManifest (no T0 fields), service input types

Privacy: persona_id / author_persona_id marked T0 in schema + types.
Only t2_alias_commitment (T2) appears in dvn_receipts.
All tables RLS-gated (service_role only) per spine rules.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Added | `supabase/migrations/20260513010000_content_qubes_schema.sql` |
| Added | `types/contentQube.ts` |

## Stats

 2 files changed, 626 insertions(+)
