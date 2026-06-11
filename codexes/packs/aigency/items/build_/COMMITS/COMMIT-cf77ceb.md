# Commit Brief: `cf77ceb` — uploads: portable persona-upload service (storage + metadata + indexer) + API + drawer + compose-strip icon

| Field | Value |
|-------|-------|
| SHA | [`cf77ceb`](https://github.com/Kn0w-1/AigentZBeta/commit/cf77ceb31a49c2f5044624a913eabb99a4c3783c) |
| Author | Claude |
| Date | 2026-05-27T20:24:12Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
uploads: portable persona-upload service (storage + metadata + indexer) + API + drawer + compose-strip icon
```

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/uploads/[id]/route.ts` |
| Added | `app/api/uploads/route.ts` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/copilot/ComposeQuickActionsStrip.tsx` |
| Added | `components/metame/uploads/UploadDrawer.tsx` |
| Added | `services/uploads/personaUploadService.ts` |
| Added | `services/uploads/supabaseUploadAdapter.ts` |
| Added | `services/uploads/uploadIndexer.ts` |
| Added | `supabase/migrations/20260527000000_persona_uploads.sql` |

## Stats

 9 files changed, 1348 insertions(+), 14 deletions(-)
