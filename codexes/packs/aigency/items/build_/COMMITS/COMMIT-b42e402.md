# Commit Brief: `b42e402` — chief-of-staff workstream: Layer 3 content gating + endpoint enforcement + recommender admin context

| Field | Value |
|-------|-------|
| SHA | [`b42e402`](https://github.com/Kn0w-1/AigentZBeta/commit/b42e402b7755167a6342366ffa4539bcf0c84d1e) |
| Author | Claude |
| Date | 2026-05-26T11:28:18Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
chief-of-staff workstream: Layer 3 content gating + endpoint enforcement + recommender admin context

Three-into-one workstream per operator request 2026-05-26. Completes
the per-cartridge admin spine extension by closing the remaining
three loops: content-tier authorization protocol, server-side
endpoint enforcement, and aigentMe recommender context enrichment.

Layer 3 — ContentQube admin-cartridge gating
--------------------------------------------
types/access.ts — extends ContentGatingDescriptor JSDoc to document
the 'admin-cartridge:<slug>' credential class as a recognized gating
contract. No type signature change required: the credential field
was already typed as `string`, so the protocol works natively.
credentialMatchesCartridgeFlag (extended earlier this session) is
the resolver, evaluateAccess routes it through the standard credential
branch, and credentialRequiresExternalVerifier excludes the
admin-cartridge prefix from the cohort/token external-verifier path.

tests/layer3-admin-cartridge-gating.test.ts (NEW) — 6 canary tests
locking the end-to-end protocol through evaluateAccess:
  - tenant-admin of KNYT reading admin-cartridge:knyt-codex content
    → allow
  - global uber-admin reading admin-cartridge:<any> → allow (uber
    override)
  - tenant-admin of KNYT reading admin-cartridge:marketa → deny
  - non-admin reading any admin-cartridge content → deny
  - partner credential NOT satisfied by admin-cartridge grants
    (namespace bleed defense)
  - malformed credential ('admin-cartridge:' with empty slug) →
    deny even for global admins (fail-closed at the credential
    parser)

Server-side per-cartridge admin endpoint enforcement
----------------------------------------------------
services/access/requireCartridgeAdmin.ts (NEW) — canonical helper:
  - isCartridgeAdmin(context, cartridgeSlug): boolean predicate
  - requireCartridgeAdmin(request, cartridgeSlug): async route guard
    that resolves persona via spine, returns either the ActivePersonaContext
    OR a NextResponse 401/403. Pattern:
        const gate = await requireCartridgeAdmin(req, 'knyt-codex');
        if (gate instanceof NextResponse) return gate;
        // ...

Retrofitted routes (representative set):
  - /api/admin/knyt/tasks-rewards (GET, PATCH) — replaced inline
    `assertAdmin` that gated on global isAdmin only. KNYT tenant-admins
    can now manage tasks/rewards.
  - /api/admin/knyt/sku-config (GET, PATCH) — added gate (was ungated).
  - /api/admin/qriptopian/content (GET) — added gate (was UNGATED;
    any caller could read every Qriptopian smart_content_qubes row).
  - /api/admin/marketa/operator-propose (POST) — added gate (was
    UNGATED; any authenticated caller could spoof an operator-authored
    campaign proposal straight into the approval queue).

Other admin endpoints touching global concerns (activity-receipts/
finalize, codex/canonical, system/rate-limits, activations/grant,
diag/*) stay on cartridgeFlags.isAdmin — those are correctly global-
scoped and don't need per-cartridge granularity.

tests/require-cartridge-admin.test.ts (NEW) — 5 canary tests on the
predicate. HTTP wrapper exercised via the layer3-admin canary's
evaluateAccess path.

aigentMe recommender admin-tier context (the chief-of-staff payoff)
-------------------------------------------------------------------
services/orchestration/adminContextSummarizer.ts (NEW) — single
hook point for cartridge-specific admin-tier signal summaries.
Returns a free-form string folded into the existing liveContext seam
(same channel the Capability Gateway preflight uses) so the LLM
rerank prompt receives admin-tier context without adding a new
prompt channel.

v1 summarizers (one per known cartridge admin surface):
  - summarizeKnyt: recent intent queue depth (pending approval / in
    progress counts)
  - summarizeMarketa: active campaign / outreach intents
  - summarizeQripto: editorial activity events from receipts

Each summarizer wraps its data source in try/catch and returns null
on any failure — recommender keeps working with whatever signal it
has. Global admins get a single-line scope acknowledgement (per-
cartridge fanout skipped to keep prompt budget compact); explicit
per-cartridge grants get per-slug summaries.

Privacy posture: NEVER include T0 ids in summary text (LLM may log,
receipt may surface). All outputs are slug-named cartridges and
integer counts only.

Wire-in routes:
  - /api/assistant/brief: fold admin summary into liveContext
    alongside preflight summary
  - /api/assistant/move-forward: same
  - /api/assistant/specialist-recommend: same

All three concatenate [preflightSummary, adminSummary] with double-
newline so the LLM sees both signals as separate paragraphs.

Net visibility outcomes
-----------------------
- Admin-tier content (when seeded with gating_credential =
  'admin-cartridge:<slug>') gates correctly through evaluateAccess
- /api/admin/<cartridge>/* endpoints honour per-cartridge grants —
  tenant-admins no longer get 403s from endpoints UI gates allow
- aigentMe brief / move-forward / specialist recommendations bias
  toward chief-of-staff moves for admins (review queues, partner
  ops, content-pipeline state) when LLM rerank is enabled

33/33 admin-related canaries pass (layer3 6/6 + require-cartridge-admin
5/5 + cartridge-admin-grants 12/12 + spine-admin-cartridges 10/10).
```

## Body

Three-into-one workstream per operator request 2026-05-26. Completes
the per-cartridge admin spine extension by closing the remaining
three loops: content-tier authorization protocol, server-side
endpoint enforcement, and aigentMe recommender context enrichment.

Layer 3 — ContentQube admin-cartridge gating
--------------------------------------------
types/access.ts — extends ContentGatingDescriptor JSDoc to document
the 'admin-cartridge:<slug>' credential class as a recognized gating
contract. No type signature change required: the credential field
was already typed as `string`, so the protocol works natively.
credentialMatchesCartridgeFlag (extended earlier this session) is
the resolver, evaluateAccess routes it through the standard credential
branch, and credentialRequiresExternalVerifier excludes the
admin-cartridge prefix from the cohort/token external-verifier path.

tests/layer3-admin-cartridge-gating.test.ts (NEW) — 6 canary tests
locking the end-to-end protocol through evaluateAccess:
  - tenant-admin of KNYT reading admin-cartridge:knyt-codex content
    → allow
  - global uber-admin reading admin-cartridge:<any> → allow (uber
    override)
  - tenant-admin of KNYT reading admin-cartridge:marketa → deny
  - non-admin reading any admin-cartridge content → deny
  - partner credential NOT satisfied by admin-cartridge grants
    (namespace bleed defense)
  - malformed credential ('admin-cartridge:' with empty slug) →
    deny even for global admins (fail-closed at the credential
    parser)

Server-side per-cartridge admin endpoint enforcement
----------------------------------------------------
services/access/requireCartridgeAdmin.ts (NEW) — canonical helper:
  - isCartridgeAdmin(context, cartridgeSlug): boolean predicate
  - requireCartridgeAdmin(request, cartridgeSlug): async route guard
    that resolves persona via spine, returns either the ActivePersonaContext
    OR a NextResponse 401/403. Pattern:
        const gate = await requireCartridgeAdmin(req, 'knyt-codex');
        if (gate instanceof NextResponse) return gate;
        // ...

Retrofitted routes (representative set):
  - /api/admin/knyt/tasks-rewards (GET, PATCH) — replaced inline
    `assertAdmin` that gated on global isAdmin only. KNYT tenant-admins
    can now manage tasks/rewards.
  - /api/admin/knyt/sku-config (GET, PATCH) — added gate (was ungated).
  - /api/admin/qriptopian/content (GET) — added gate (was UNGATED;
    any caller could read every Qriptopian smart_content_qubes row).
  - /api/admin/marketa/operator-propose (POST) — added gate (was
    UNGATED; any authenticated caller could spoof an operator-authored
    campaign proposal straight into the approval queue).

Other admin endpoints touching global concerns (activity-receipts/
finalize, codex/canonical, system/rate-limits, activations/grant,
diag/*) stay on cartridgeFlags.isAdmin — those are correctly global-
scoped and don't need per-cartridge granularity.

tests/require-cartridge-admin.test.ts (NEW) — 5 canary tests on the
predicate. HTTP wrapper exercised via the layer3-admin canary's
evaluateAccess path.

aigentMe recommender admin-tier context (the chief-of-staff payoff)
-------------------------------------------------------------------
services/orchestration/adminContextSummarizer.ts (NEW) — single
hook point for cartridge-specific admin-tier signal summaries.
Returns a free-form string folded into the existing liveContext seam
(same channel the Capability Gateway preflight uses) so the LLM
rerank prompt receives admin-tier context without adding a new
prompt channel.

v1 summarizers (one per known cartridge admin surface):
  - summarizeKnyt: recent intent queue depth (pending approval / in
    progress counts)
  - summarizeMarketa: active campaign / outreach intents
  - summarizeQripto: editorial activity events from receipts

Each summarizer wraps its data source in try/catch and returns null
on any failure — recommender keeps working with whatever signal it
has. Global admins get a single-line scope acknowledgement (per-
cartridge fanout skipped to keep prompt budget compact); explicit
per-cartridge grants get per-slug summaries.

Privacy posture: NEVER include T0 ids in summary text (LLM may log,
receipt may surface). All outputs are slug-named cartridges and
integer counts only.

Wire-in routes:
  - /api/assistant/brief: fold admin summary into liveContext
    alongside preflight summary
  - /api/assistant/move-forward: same
  - /api/assistant/specialist-recommend: same

All three concatenate [preflightSummary, adminSummary] with double-
newline so the LLM sees both signals as separate paragraphs.

Net visibility outcomes
-----------------------
- Admin-tier content (when seeded with gating_credential =
  'admin-cartridge:<slug>') gates correctly through evaluateAccess
- /api/admin/<cartridge>/* endpoints honour per-cartridge grants —
  tenant-admins no longer get 403s from endpoints UI gates allow
- aigentMe brief / move-forward / specialist recommendations bias
  toward chief-of-staff moves for admins (review queues, partner
  ops, content-pipeline state) when LLM rerank is enabled

33/33 admin-related canaries pass (layer3 6/6 + require-cartridge-admin
5/5 + cartridge-admin-grants 12/12 + spine-admin-cartridges 10/10).

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/admin/knyt/sku-config/route.ts` |
| Modified | `app/api/admin/knyt/tasks-rewards/route.ts` |
| Modified | `app/api/admin/marketa/operator-propose/route.ts` |
| Modified | `app/api/admin/qriptopian/content/route.ts` |
| Modified | `app/api/assistant/brief/route.ts` |
| Modified | `app/api/assistant/move-forward/route.ts` |
| Modified | `app/api/assistant/specialist-recommend/route.ts` |
| Added | `services/access/requireCartridgeAdmin.ts` |
| Added | `services/orchestration/adminContextSummarizer.ts` |
| Added | `tests/layer3-admin-cartridge-gating.test.ts` |
| Added | `tests/require-cartridge-admin.test.ts` |
| Modified | `types/access.ts` |

## Stats

 12 files changed, 565 insertions(+), 27 deletions(-)
