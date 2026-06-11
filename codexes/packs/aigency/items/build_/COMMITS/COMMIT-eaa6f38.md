# Commit Brief: `eaa6f38` — feat(content-qube): Phase 3 — registry VIEW + GET /api/registry/content-qube/[id]

| Field | Value |
|-------|-------|
| SHA | [`eaa6f38`](https://github.com/Kn0w-1/AigentZBeta/commit/eaa6f3835bcd63379e272a04fbce92062cd53f2c) |
| Author | Claude |
| Date | 2026-05-13T19:16:08Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
feat(content-qube): Phase 3 — registry VIEW + GET /api/registry/content-qube/[id]

- supabase/migrations/20260513020000_content_qube_registry_view.sql
  v_content_qube_registry: denormalised read view joining content_qubes
  with access policy, primary storage, edition aggregates, and codex
  binding slugs. Server-internal; T0 fields not exposed in view shape.
- app/api/registry/content-qube/[id]/route.ts
  Reads the view and emits ContentQubeDisplayManifest (T1-safe) +
  editionSummary + codexSlugs. Storage URLs intentionally withheld —
  delivery continues via existing proxy routes. persona_owns is false
  until Phase 4 wires services/access/evaluateAccess.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n
```

## Body

- supabase/migrations/20260513020000_content_qube_registry_view.sql
  v_content_qube_registry: denormalised read view joining content_qubes
  with access policy, primary storage, edition aggregates, and codex
  binding slugs. Server-internal; T0 fields not exposed in view shape.
- app/api/registry/content-qube/[id]/route.ts
  Reads the view and emits ContentQubeDisplayManifest (T1-safe) +
  editionSummary + codexSlugs. Storage URLs intentionally withheld —
  delivery continues via existing proxy routes. persona_owns is false
  until Phase 4 wires services/access/evaluateAccess.

https://claude.ai/code/session_01Ths4F8mcdYjDcKnjxnMy9n

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Added | `app/api/registry/content-qube/[id]/route.ts` |
| Added | `supabase/migrations/20260513020000_content_qube_registry_view.sql` |

## Stats

 3 files changed, 243 insertions(+), 1 deletion(-)
