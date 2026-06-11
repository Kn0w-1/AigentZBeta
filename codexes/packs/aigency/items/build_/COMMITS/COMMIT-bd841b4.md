# Commit Brief: `bd841b4` — access requests: new non-admin cartridge_access class + rename to alpha access

| Field | Value |
|-------|-------|
| SHA | [`bd841b4`](https://github.com/Kn0w-1/AigentZBeta/commit/bd841b46796d38c0e154e0e7a19fec59e8c7c6a8) |
| Author | Claude |
| Date | 2026-05-26T18:22:04Z |
| Branch | dev (direct push) |
| Type | `refactor` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
access requests: new non-admin cartridge_access class + rename to alpha access

Operator feedback: the alpha flow conflated 'I want to use this
cartridge' with 'I want to administer this cartridge'. Most requesters
just want runtime visibility — they shouldn't be asking for admin
scope.

Schema (migration 20260526020000):
- ALTER admin_access_requests ADD request_type CHECK IN
  (cartridge_access | cartridge_admin | global_admin), default
  cartridge_access.
- Backfill: existing rows where requested_at predates this migration
  get 'cartridge_admin' (or 'global_admin' if null slug), preserving
  the original semantics.
- Unique-pending partial index extended to include request_type so a
  persona can have one pending access AND one pending admin request
  for the same cartridge.

Submit route (POST /api/admin/access-requests):
- Reads optional body.requestType, defaults to cartridge_access.
- Validates: global_admin requires null slug; cartridge_* requires a
  slug.
- Existing-grant short-circuit branches on type — only blocks when
  the requester ALREADY holds the specific grant they're asking for.
- Persists request_type in the insert; response shape includes it.

Decide route (POST /api/admin/access-requests/[id]/decide):
- Branches on request_type at approval time.
- cartridge_access -> upserts persona_activations (status=active,
  granted_via=admin). Grants runtime visibility only; the requester
  does NOT see adminOnly tabs.
- cartridge_admin / global_admin -> writes crm_admin_roles (existing
  behaviour).
- Adds cartridgeSlugToActivationId() mapping table (parallel to the
  inverseCartridgeSlugToTenantSlugs map used for the admin paths).
- Response carries grantKind so the UI can render the right
  confirmation.

Modal copy + flow:
- Renamed 'Request admin access' -> 'Request alpha access' so the
  alpha context is up front. Default is cartridge_access; a checkbox
  surfaces the cartridge_admin path with explicit framing ('Most
  requesters don't need this'). Active cartridge label is interpolated
  into the button label and submit copy. Tooltip on Info icon
  explains alpha + access vs admin.
- Removed the global-admin option from the cartridge picker — it's a
  narrower path that should require a separate flow.

Reviewer tab:
- Header copy reflects the new dual flow ('access' vs 'admin
  privileges').
- Each request row now carries a request-type pill (emerald = access,
  amber = admin, rose = global). Pill title attribute carries the
  per-type explainer.

Chip label: 'Request access' -> 'Request alpha access' on the
welcome surface chip.

PII posture unchanged — full email + display label visible to global
admin reviewers per the consent backlog (the alpha PII rule).
```

## Body

Operator feedback: the alpha flow conflated 'I want to use this
cartridge' with 'I want to administer this cartridge'. Most requesters
just want runtime visibility — they shouldn't be asking for admin
scope.

Schema (migration 20260526020000):
- ALTER admin_access_requests ADD request_type CHECK IN
  (cartridge_access | cartridge_admin | global_admin), default
  cartridge_access.
- Backfill: existing rows where requested_at predates this migration
  get 'cartridge_admin' (or 'global_admin' if null slug), preserving
  the original semantics.
- Unique-pending partial index extended to include request_type so a
  persona can have one pending access AND one pending admin request
  for the same cartridge.

Submit route (POST /api/admin/access-requests):
- Reads optional body.requestType, defaults to cartridge_access.
- Validates: global_admin requires null slug; cartridge_* requires a
  slug.
- Existing-grant short-circuit branches on type — only blocks when
  the requester ALREADY holds the specific grant they're asking for.
- Persists request_type in the insert; response shape includes it.

Decide route (POST /api/admin/access-requests/[id]/decide):
- Branches on request_type at approval time.
- cartridge_access -> upserts persona_activations (status=active,
  granted_via=admin). Grants runtime visibility only; the requester
  does NOT see adminOnly tabs.
- cartridge_admin / global_admin -> writes crm_admin_roles (existing
  behaviour).
- Adds cartridgeSlugToActivationId() mapping table (parallel to the
  inverseCartridgeSlugToTenantSlugs map used for the admin paths).
- Response carries grantKind so the UI can render the right
  confirmation.

Modal copy + flow:
- Renamed 'Request admin access' -> 'Request alpha access' so the
  alpha context is up front. Default is cartridge_access; a checkbox
  surfaces the cartridge_admin path with explicit framing ('Most
  requesters don't need this'). Active cartridge label is interpolated
  into the button label and submit copy. Tooltip on Info icon
  explains alpha + access vs admin.
- Removed the global-admin option from the cartridge picker — it's a
  narrower path that should require a separate flow.

Reviewer tab:
- Header copy reflects the new dual flow ('access' vs 'admin
  privileges').
- Each request row now carries a request-type pill (emerald = access,
  amber = admin, rose = global). Pill title attribute carries the
  per-type explainer.

Chip label: 'Request access' -> 'Request alpha access' on the
welcome surface chip.

PII posture unchanged — full email + display label visible to global
admin reviewers per the consent backlog (the alpha PII rule).

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/admin/access-requests/[id]/decide/route.ts` |
| Modified | `app/api/admin/access-requests/route.ts` |
| Modified | `app/triad/components/codex/tabs/AdminAccessRequestsTab.tsx` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/admin/RequestAdminAccessButton.tsx` |
| Modified | `data/codex-configs.ts` |
| Added | `supabase/migrations/20260526020000_admin_access_requests_request_type.sql` |

## Stats

 7 files changed, 333 insertions(+), 61 deletions(-)
