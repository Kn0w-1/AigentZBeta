# Commit Brief: `acefbea` — admin access requests: persona-initiated request workflow + global admin review tab

| Field | Value |
|-------|-------|
| SHA | [`acefbea`](https://github.com/Kn0w-1/AigentZBeta/commit/acefbea22fa72f88e305acfca183efdb0a561c42) |
| Author | Claude |
| Date | 2026-05-26T16:50:22Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
admin access requests: persona-initiated request workflow + global admin review tab

Phase C of the refinements pass.

Persona without any admin grants can now self-request admin access via
a 'Request admin access' affordance on the aigentMe welcome surface
(top-right; hidden the moment any grant exists). Submission writes a
'pending' row in admin_access_requests. Global admins review and decide
inside the new metaMe Cartridge -> Admin -> Access Requests tab, where
each row is inline-enriched with T1-safe CRM context (existing admin
grants, active activations, investor flag) so the reviewer doesn't have
to round-trip CRM. Approve writes the matching crm_admin_roles row
(platform_super_admin for null slug, tenant_super_admin otherwise);
deny just records the reason.

Migration: 20260526000000_admin_access_requests.sql — service-role-only
RLS, unique partial index on (persona_id, requested_cartridge_slug)
WHERE status = 'pending' so duplicates are prevented but re-requests
after denial are allowed.

Routes:
- POST /api/admin/access-requests — submit (spine-gated)
- GET /api/admin/access-requests — list + enrichment (global-admin only)
- POST /api/admin/access-requests/[id]/decide — approve | deny

UI:
- AdminAccessRequestsTab (new) — filter chips, expandable rows,
  decision form, enrichment grid
- RequestAdminAccessButton — modal with cartridge dropdown +
  justification textarea; gated render on adminGrants empty

Spine: persona_id and auth_profile_id never leave the server (T0);
response surface strips them. Caller always resolved via the spine,
never trusted from a client claim.

aigentMe persona prompt updated to know the workflow exists so the
copilot can point users to it when they mention needing admin access.

Followup (NOT in this commit, captured for next phase): broader CRM
identity-data enrichment (link personaId, DIDQube identities,
SmartWallet ownership, AIQS, etc. inside the CRM record itself).
That's a separate workstream requiring CRM schema changes.

Migration MUST be applied in Supabase before the route is exercised
in dev/staging.
```

## Body

Phase C of the refinements pass.

Persona without any admin grants can now self-request admin access via
a 'Request admin access' affordance on the aigentMe welcome surface
(top-right; hidden the moment any grant exists). Submission writes a
'pending' row in admin_access_requests. Global admins review and decide
inside the new metaMe Cartridge -> Admin -> Access Requests tab, where
each row is inline-enriched with T1-safe CRM context (existing admin
grants, active activations, investor flag) so the reviewer doesn't have
to round-trip CRM. Approve writes the matching crm_admin_roles row
(platform_super_admin for null slug, tenant_super_admin otherwise);
deny just records the reason.

Migration: 20260526000000_admin_access_requests.sql — service-role-only
RLS, unique partial index on (persona_id, requested_cartridge_slug)
WHERE status = 'pending' so duplicates are prevented but re-requests
after denial are allowed.

Routes:
- POST /api/admin/access-requests — submit (spine-gated)
- GET /api/admin/access-requests — list + enrichment (global-admin only)
- POST /api/admin/access-requests/[id]/decide — approve | deny

UI:
- AdminAccessRequestsTab (new) — filter chips, expandable rows,
  decision form, enrichment grid
- RequestAdminAccessButton — modal with cartridge dropdown +
  justification textarea; gated render on adminGrants empty

Spine: persona_id and auth_profile_id never leave the server (T0);
response surface strips them. Caller always resolved via the spine,
never trusted from a client claim.

aigentMe persona prompt updated to know the workflow exists so the
copilot can point users to it when they mention needing admin access.

Followup (NOT in this commit, captured for next phase): broader CRM
identity-data enrichment (link personaId, DIDQube identities,
SmartWallet ownership, AIQS, etc. inside the CRM record itself).
That's a separate workstream requiring CRM schema changes.

Migration MUST be applied in Supabase before the route is exercised
in dev/staging.

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/admin/access-requests/[id]/decide/route.ts` |
| Added | `app/api/admin/access-requests/route.ts` |
| Modified | `app/data/personas.ts` |
| Modified | `app/triad/components/codex/TabRenderer.tsx` |
| Added | `app/triad/components/codex/tabs/AdminAccessRequestsTab.tsx` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Added | `components/metame/admin/RequestAdminAccessButton.tsx` |
| Modified | `data/codex-configs.ts` |
| Added | `supabase/migrations/20260526000000_admin_access_requests.sql` |

## Stats

 9 files changed, 1128 insertions(+)
