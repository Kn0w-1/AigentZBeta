# Commit Brief: `b43e73f` — attach supabase bearer token to /api/wallet/tasks fetches

| Field | Value |
|-------|-------|
| SHA | [`b43e73f`](https://github.com/Kn0w-1/AigentZBeta/commit/b43e73fa85707b5cdea6d996a790e804fb974daa) |
| Author | Claude |
| Date | 2026-05-19T21:21:15Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
attach supabase bearer token to /api/wallet/tasks fetches

The spine's getCallerIdentityContext (services/wallet/personaRepo.ts)
authenticates via Authorization: Bearer <supabase-jwt> and does not read
cookies, so credentials: 'include' alone caused every /api/wallet/tasks*
fetch to 401 even for signed-in users. Mirror the auth-header pattern
the same files already use for /api/wallet/persona/*: read the session
access_token via getSupabaseBrowserClient().auth.getSession() and pass
it as Authorization on each call.

Fixed call sites:
- SmartWalletDrawer.tsx: tasks (drawer load), share-link, tasks
  (post-redeem refresh), track-click
- KnytTab.tsx: tasks (Order HUD load), tasks (post-redeem refresh)
```

## Body

The spine's getCallerIdentityContext (services/wallet/personaRepo.ts)
authenticates via Authorization: Bearer <supabase-jwt> and does not read
cookies, so credentials: 'include' alone caused every /api/wallet/tasks*
fetch to 401 even for signed-in users. Mirror the auth-header pattern
the same files already use for /api/wallet/persona/*: read the session
access_token via getSupabaseBrowserClient().auth.getSession() and pass
it as Authorization on each call.

Fixed call sites:
- SmartWalletDrawer.tsx: tasks (drawer load), share-link, tasks
  (post-redeem refresh), track-click
- KnytTab.tsx: tasks (Order HUD load), tasks (post-redeem refresh)

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/components/content/SmartWalletDrawer.tsx` |
| Modified | `app/triad/components/codex/tabs/KnytTab.tsx` |

## Stats

 2 files changed, 59 insertions(+), 23 deletions(-)
