# Commit Brief: `cef2f1b` — add save to myCanvas for origin + derived Experience Qubes

| Field | Value |
|-------|-------|
| SHA | [`cef2f1b`](https://github.com/Kn0w-1/AigentZBeta/commit/cef2f1b99f4c6a6aed47c7b5ac5ee994f6567291) |
| Author | Claude |
| Date | 2026-05-22T04:29:51Z |
| Branch | dev (direct push) |
| Type | `feat` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
add save to myCanvas for origin + derived Experience Qubes

RemixDialog preview footer gains a "Save to myCanvas" button (violet,
both modal and inline variants). Clicking it saves two canvas entries
in parallel via POST /api/mycanvas/entries:
  - experience_derived: full article body + imageUrl + skill in meta_json
  - experience_origin: title reference + experienceId in meta_json
    (only when sourceExperienceId is present)

Button shows spinner while saving, flips to "Saved ✓" on success,
resets when the user clicks Redo.

MyCanvasTab right panel now branches on entryType:
  - experience_derived → read-only rendered capsule (image + prose)
  - experience_origin  → reference card with amber "Origin Experience" badge
  - note               → existing freeform editor (unchanged)

Sidebar chips gain Cpu icon (amber) for origin entries, Sparkles icon
(violet) for derived entries so the two types are visually distinct.

DB migration adds entry_type (CHECK 'note'|'experience_origin'|
'experience_derived') and meta_json (jsonb) columns to mycanvas_entries
with safe defaults so existing note rows are unaffected.

canvasService + POST route updated to accept and persist the new fields.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym
```

## Body

RemixDialog preview footer gains a "Save to myCanvas" button (violet,
both modal and inline variants). Clicking it saves two canvas entries
in parallel via POST /api/mycanvas/entries:
  - experience_derived: full article body + imageUrl + skill in meta_json
  - experience_origin: title reference + experienceId in meta_json
    (only when sourceExperienceId is present)

Button shows spinner while saving, flips to "Saved ✓" on success,
resets when the user clicks Redo.

MyCanvasTab right panel now branches on entryType:
  - experience_derived → read-only rendered capsule (image + prose)
  - experience_origin  → reference card with amber "Origin Experience" badge
  - note               → existing freeform editor (unchanged)

Sidebar chips gain Cpu icon (amber) for origin entries, Sparkles icon
(violet) for derived entries so the two types are visually distinct.

DB migration adds entry_type (CHECK 'note'|'experience_origin'|
'experience_derived') and meta_json (jsonb) columns to mycanvas_entries
with safe defaults so existing note rows are unaffected.

canvasService + POST route updated to accept and persist the new fields.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/mycanvas/entries/route.ts` |
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |
| Modified | `services/mycanvas/canvasService.ts` |
| Added | `supabase/migrations/20260523020000_mycanvas_experience_types.sql` |

## Stats

 5 files changed, 270 insertions(+), 8 deletions(-)
