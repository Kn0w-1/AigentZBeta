# Commit Brief: `ba19445` — Forward Supabase access token from registry browser page

| Field | Value |
|-------|-------|
| SHA | [`ba19445`](https://github.com/Kn0w-1/AigentZBeta/commit/ba194459bf7c310eb33c2f133ec1918912eae104) |
| Author | Claude |
| Date | 2026-05-14T20:30:32Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Forward Supabase access token from registry browser page

The /registry/content-qubes page was failing with 401 because browser
fetch() doesn't auto-attach the Supabase Bearer token, and the
/api/registry/content-qube/browse route uses getActivePersona(req)
which requires Authorization: Bearer <jwt>.

Fix mirrors the CodexUploadModal pattern: call
getSupabaseBrowserClient().auth.getSession() and forward the
access_token in the Authorization header.

The admin gate (cartridgeFlags.isAdmin === true) is still enforced
server-side; this commit only fixes the credential transport so the
gate can evaluate against the real persona instead of unauthenticated.
```

## Body

The /registry/content-qubes page was failing with 401 because browser
fetch() doesn't auto-attach the Supabase Bearer token, and the
/api/registry/content-qube/browse route uses getActivePersona(req)
which requires Authorization: Bearer <jwt>.

Fix mirrors the CodexUploadModal pattern: call
getSupabaseBrowserClient().auth.getSession() and forward the
access_token in the Authorization header.

The admin gate (cartridgeFlags.isAdmin === true) is still enforced
server-side; this commit only fixes the credential transport so the
gate can evaluate against the real persona instead of unauthenticated.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/(shell)/registry/content-qubes/page.tsx` |

## Stats

 2 files changed, 23 insertions(+), 9 deletions(-)
