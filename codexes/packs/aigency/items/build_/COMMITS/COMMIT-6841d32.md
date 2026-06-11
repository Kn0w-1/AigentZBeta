# Commit Brief: `6841d32` — uploads: email attachment use-kind + Marketa Mailjet attachments + Gmail picker UI + iQube embed stub

| Field | Value |
|-------|-------|
| SHA | [`6841d32`](https://github.com/Kn0w-1/AigentZBeta/commit/6841d32d3ec789f28f1119de5a26998c1a7b5549) |
| Author | Claude |
| Date | 2026-05-27T21:45:11Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
uploads: email attachment use-kind + Marketa Mailjet attachments + Gmail picker UI + iQube embed stub
```

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/assistant/create-artifact/route.ts` |
| Added | `app/api/uploads/[id]/iqube-embed/route.ts` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/connections/ComposeGmailDraftModal.tsx` |
| Modified | `components/metame/connections/ComposeMarketaEmailModal.tsx` |
| Added | `components/metame/uploads/UploadAttachmentPicker.tsx` |
| Modified | `services/marketa/marketaConnector.ts` |
| Added | `services/uploads/iqubeUploadEmbed.ts` |
| Modified | `services/uploads/personaUploadService.ts` |
| Added | `services/uploads/uploadAttachmentHelper.ts` |
| Added | `supabase/migrations/20260527010000_persona_uploads_attachment_kinds.sql` |

## Stats

 11 files changed, 581 insertions(+), 15 deletions(-)
