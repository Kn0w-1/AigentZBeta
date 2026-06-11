# Commit Brief: `fb9ec3b` — diag: read-only persona-resolution dump for dele@metame.com devagent override debug

| Field | Value |
|-------|-------|
| SHA | [`fb9ec3b`](https://github.com/Kn0w-1/AigentZBeta/commit/fb9ec3b9ff67f89f24c8253e5f572a901e854eac) |
| Author | Claude |
| Date | 2026-05-22T22:27:45Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
diag: read-only persona-resolution dump for dele@metame.com devagent override debug

User's certainty that devagent is their YOUNGEST persona invalidates
all my 'created_at backdating' hypotheses. The actual mechanism must
be something else — status filter? merge-link bringing in personas
from another profile? caching? — and without DB access I can't see
the data to confirm.

This endpoint dumps the exact rows the resolver sees, in the exact
order it sees them. Admin-gated (same crm_admin_roles check as other
/api/admin endpoints). Returns four lenses on the data:

  • personas_active_asc — what listOwnedPersonas actually returns
    inside getActivePersona; index [0] wins step 4. If this list has
    devagent at index 0 we know step 4 is firing AND we know why
    (either devagent's created_at really is oldest in the merged
    set, or it's the only active one).

  • personas_active_desc — same rows opposite sort. Visual aid to
    confirm which persona is genuinely newest by created_at.

  • personas_all_statuses_asc — drops the status='active' filter so
    we can see if archived/inactive rows exist. If devagent is the
    only active one, the resolver picks it trivially regardless of
    timestamps.

  • crmAuthProfileLinkRows — the merge edges. Shows whether devagent
    is under the caller's primary auth profile or a separately-merged
    one.

Plus the resolver's actual output (personaId + source) and a
one-line hint comparing it to personas_active_asc[0] to tell us at
a glance whether step 4 fired or 1-3 did.

Never returns personaSessionToken or any T0 material. Read-only.
Hit it once and we'll know the actual mechanism, then I can ship the
right fix without guessing.
```

## Body

User's certainty that devagent is their YOUNGEST persona invalidates
all my 'created_at backdating' hypotheses. The actual mechanism must
be something else — status filter? merge-link bringing in personas
from another profile? caching? — and without DB access I can't see
the data to confirm.

This endpoint dumps the exact rows the resolver sees, in the exact
order it sees them. Admin-gated (same crm_admin_roles check as other
/api/admin endpoints). Returns four lenses on the data:

  • personas_active_asc — what listOwnedPersonas actually returns
    inside getActivePersona; index [0] wins step 4. If this list has
    devagent at index 0 we know step 4 is firing AND we know why
    (either devagent's created_at really is oldest in the merged
    set, or it's the only active one).

  • personas_active_desc — same rows opposite sort. Visual aid to
    confirm which persona is genuinely newest by created_at.

  • personas_all_statuses_asc — drops the status='active' filter so
    we can see if archived/inactive rows exist. If devagent is the
    only active one, the resolver picks it trivially regardless of
    timestamps.

  • crmAuthProfileLinkRows — the merge edges. Shows whether devagent
    is under the caller's primary auth profile or a separately-merged
    one.

Plus the resolver's actual output (personaId + source) and a
one-line hint comparing it to personas_active_asc[0] to tell us at
a glance whether step 4 fired or 1-3 did.

Never returns personaSessionToken or any T0 material. Read-only.
Hit it once and we'll know the actual mechanism, then I can ship the
right fix without guessing.

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/admin/diag/persona-resolution/route.ts` |

## Stats

 1 file changed, 176 insertions(+)
