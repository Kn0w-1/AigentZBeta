# Commit Brief: `a5e291b` — discard: route refund + quota stamp to the CREATOR persona, not the caller

| Field | Value |
|-------|-------|
| SHA | [`a5e291b`](https://github.com/Kn0w-1/AigentZBeta/commit/a5e291b0c37ba7a721b60c17f095f05fe5d92d03) |
| Author | Claude |
| Date | 2026-05-23T07:44:59Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
discard: route refund + quota stamp to the CREATOR persona, not the caller

Caught while answering a privacy/disclosure question about the
just-added callerOwnsCreator relaxation. The discard route was using
the resolving personaId (caller) for the refund AND for the quota
stamp:
  - creditQc(supabase, personaId, ...)  → caller's wallet
  - upsert community_content_quotas { persona_id: personaId }  → caller's daily-refund stamp

With strict equality (caller == creator), these were identical. With
the auth-profile-level relaxation, devagent could discard marketa's
draft and the refund would land in devagent's wallet, leaking value
between personas. The ownership check is about WHO can trigger the
discard; WHO bears the financial outcome must always be the creator.

Fix: introduce financialPersonaId = content.creator_persona_id and
route the refund + the quota stamp through that. Quota check is also
re-run against the creator's quota row when caller != creator, so the
'daily refund already used' limit applies to the right persona.

Side-effect-bearing operations now stay tightly bound to the persona
that originally paid for the Q¢ generation. Cross-persona linkage is
still observed by the operator (server-side), but never leaks T0
data to the browser, the community feed, receipts, or chain.
```

## Body

Caught while answering a privacy/disclosure question about the
just-added callerOwnsCreator relaxation. The discard route was using
the resolving personaId (caller) for the refund AND for the quota
stamp:
  - creditQc(supabase, personaId, ...)  → caller's wallet
  - upsert community_content_quotas { persona_id: personaId }  → caller's daily-refund stamp

With strict equality (caller == creator), these were identical. With
the auth-profile-level relaxation, devagent could discard marketa's
draft and the refund would land in devagent's wallet, leaking value
between personas. The ownership check is about WHO can trigger the
discard; WHO bears the financial outcome must always be the creator.

Fix: introduce financialPersonaId = content.creator_persona_id and
route the refund + the quota stamp through that. Quota check is also
re-run against the creator's quota row when caller != creator, so the
'daily refund already used' limit applies to the right persona.

Side-effect-bearing operations now stay tightly bound to the persona
that originally paid for the Q¢ generation. Cross-persona linkage is
still observed by the operator (server-side), but never leaks T0
data to the browser, the community feed, receipts, or chain.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/[id]/discard/route.ts` |

## Stats

 1 file changed, 26 insertions(+), 5 deletions(-)
