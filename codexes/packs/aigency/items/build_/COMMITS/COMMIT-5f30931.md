# Commit Brief: `5f30931` — myCanvas: full origin capture + remix/share/invite/publish actions

| Field | Value |
|-------|-------|
| SHA | [`5f30931`](https://github.com/Kn0w-1/AigentZBeta/commit/5f309312eb7543cf46bb85a7bee1b36e4e24a48e) |
| Author | Claude |
| Date | 2026-05-22T06:32:42Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
myCanvas: full origin capture + remix/share/invite/publish actions

Origin save now captures the full source experience (image + description),
not just an ID reference. The remix runtime forwards content.coverImageUri
and content.description through RuntimeCapsuleRemixEditor → RemixDialog →
the origin canvas entry's metaJson (imageUrl) and bodyMd (description),
so MyCanvasTab renders the source capsule the same way it renders the
derived remix — same image+prose layout, not an empty reference card.

ExperienceOriginPanel and ExperienceDerivedPanel get a unified action bar:

  • Remix      — opens RemixDialog with the saved entry as source, so the
                 user can iterate a remix-of-a-remix. The dialog's existing
                 Save to myCanvas button persists the new derivative back
                 into the canvas automatically.
  • Share      — native share API + clipboard fallback for the entry title.
  • Invite     — reuses the existing mycanvas_invites stub flow (lifted
                 into a reusable InviteBar so notes and experiences share it).
  • Publish    — derived entries only; calls the existing community-content
                 publish endpoint via metaJson.contentId, flipping the row
                 to 'shared' so it surfaces in KNYT / Qriptopian community
                 tabs. Button shows spinner→Published checkmark on success,
                 surfaces server error inline on failure.

Origin entries get the same Remix/Share/Invite controls but no Publish
(there's nothing user-generated to publish — Publish belongs to the
derived content row).

Defensive json parse in submit(): server fields now go through String()
/ typeof guards so a malformed 200 body can't crash the preview state.
```

## Body

Origin save now captures the full source experience (image + description),
not just an ID reference. The remix runtime forwards content.coverImageUri
and content.description through RuntimeCapsuleRemixEditor → RemixDialog →
the origin canvas entry's metaJson (imageUrl) and bodyMd (description),
so MyCanvasTab renders the source capsule the same way it renders the
derived remix — same image+prose layout, not an empty reference card.

ExperienceOriginPanel and ExperienceDerivedPanel get a unified action bar:

  • Remix      — opens RemixDialog with the saved entry as source, so the
                 user can iterate a remix-of-a-remix. The dialog's existing
                 Save to myCanvas button persists the new derivative back
                 into the canvas automatically.
  • Share      — native share API + clipboard fallback for the entry title.
  • Invite     — reuses the existing mycanvas_invites stub flow (lifted
                 into a reusable InviteBar so notes and experiences share it).
  • Publish    — derived entries only; calls the existing community-content
                 publish endpoint via metaJson.contentId, flipping the row
                 to 'shared' so it surfaces in KNYT / Qriptopian community
                 tabs. Button shows spinner→Published checkmark on success,
                 surfaces server error inline on failure.

Origin entries get the same Remix/Share/Invite controls but no Publish
(there's nothing user-generated to publish — Publish belongs to the
derived content row).

Defensive json parse in submit(): server fields now go through String()
/ typeof guards so a malformed 200 body can't crash the preview state.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `components/metame/MetaMeRuntimeClient.tsx` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |
| Modified | `components/metame/runtime/RuntimeCapsuleRemixEditor.tsx` |

## Stats

 4 files changed, 337 insertions(+), 35 deletions(-)
