# Commit Brief: `013a008` — fix: forward Supabase access token to /api/admin/codex/canonical

| Field | Value |
|-------|-------|
| SHA | [`013a008`](https://github.com/Kn0w-1/AigentZBeta/commit/013a008577a6a9ff1eb0ee0a2c929f92769e7764) |
| Author | Claude |
| Date | 2026-05-16T00:49:27Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix: forward Supabase access token to /api/admin/codex/canonical

The KNYT Codex Admin tab fetches /api/admin/codex/canonical from inside
an iframe context where Authorization headers are not auto-attached.
The route's getActivePersona() requires the Bearer token, so the fetch
was returning 401. Match the /registry/content-qubes page pattern:
read the Supabase session via getSupabaseBrowserClient and forward
the access_token as Authorization header.
```

## Body

The KNYT Codex Admin tab fetches /api/admin/codex/canonical from inside
an iframe context where Authorization headers are not auto-attached.
The route's getActivePersona() requires the Bearer token, so the fetch
was returning 401. Match the /registry/content-qubes page pattern:
read the Supabase session via getSupabaseBrowserClient and forward
the access_token as Authorization header.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/KnytCodexAdminTab.tsx` |

## Stats

 1 file changed, 11 insertions(+), 1 deletion(-)
