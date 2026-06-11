# Commit Brief: `d98e61e` — resolve-recipient: fuzzy handle matching + actionable error + diagnostics

| Field | Value |
|-------|-------|
| SHA | [`d98e61e`](https://github.com/Kn0w-1/AigentZBeta/commit/d98e61ede0e0347b22c72bd0f5348a60c2d5136d) |
| Author | Claude |
| Date | 2026-05-22T21:26:31Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
resolve-recipient: fuzzy handle matching + actionable error + diagnostics

Recipient resolution kept failing for inputs like 'devagent@qripto' and
did:iq DIDs because:
  • the persona table lookup only tried the EXACT full input, missing
    rows where the handle is stored as just the local part ('devagent'
    without '@qripto')
  • the agent_keys fio_handle lookup had the same problem
  • the 404 error message advertised the OLD accepted formats only —
    no mention of DIDs, persona names, or UUIDs, so users didn't know
    what other shapes to try

Changes:
  • Try three variants of every text input across both persona tables
    and agent_keys: full normalised, local-part (strip @domain), and
    loose %local-part% fuzzy match. Covers handles stored either way.
  • Updated 404 error to reflect every accepted format.
  • console.info on every lookup ('looking up' with all variants) and
    console.warn on miss — operators can grep CloudWatch to see which
    forms were tried and what column had the row, if any.

Underlying root cause of the user's symptoms isn't resolution though —
it's the server's persona-resolution defaulting to devagent for
dele@metame.com (Path B in the 2026-05-22 backlog brief). This commit
just stops the resolver from being a noisy false-negative while we
queue up Path B.
```

## Body

Recipient resolution kept failing for inputs like 'devagent@qripto' and
did:iq DIDs because:
  • the persona table lookup only tried the EXACT full input, missing
    rows where the handle is stored as just the local part ('devagent'
    without '@qripto')
  • the agent_keys fio_handle lookup had the same problem
  • the 404 error message advertised the OLD accepted formats only —
    no mention of DIDs, persona names, or UUIDs, so users didn't know
    what other shapes to try

Changes:
  • Try three variants of every text input across both persona tables
    and agent_keys: full normalised, local-part (strip @domain), and
    loose %local-part% fuzzy match. Covers handles stored either way.
  • Updated 404 error to reflect every accepted format.
  • console.info on every lookup ('looking up' with all variants) and
    console.warn on miss — operators can grep CloudWatch to see which
    forms were tried and what column had the row, if any.

Underlying root cause of the user's symptoms isn't resolution though —
it's the server's persona-resolution defaulting to devagent for
dele@metame.com (Path B in the 2026-05-22 backlog brief). This commit
just stops the resolver from being a noisy false-negative while we
queue up Path B.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/identity/resolve-recipient/route.ts` |

## Stats

 1 file changed, 59 insertions(+), 32 deletions(-)
