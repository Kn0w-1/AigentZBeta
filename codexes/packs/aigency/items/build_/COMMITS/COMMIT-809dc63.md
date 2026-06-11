# Commit Brief: `809dc63` — specialist artifacts + myWorkbench tab placeholder

| Field | Value |
|-------|-------|
| SHA | [`809dc63`](https://github.com/Kn0w-1/AigentZBeta/commit/809dc63265fd25888e539d3ae0be08d5ee6f5b98) |
| Author | Claude |
| Date | 2026-05-26T19:25:26Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
specialist artifacts + myWorkbench tab placeholder

#1 myWorkbench tab registered as a sub-tab of myCanvas. Stub mirrors
MyCanvasTab affordances until the operator confirms the distinction
between 'publish-ready output' (canvas) and 'work-in-progress'
(workbench). Listed in TabRenderer; slug 'my-workbench'; same
activation gate as myCanvas.

#5 + #6 Specialist response actions:
- Quill's editorial template now defaults its suggestedArtifacts to
  ['article', 'mycanvas-remix', 'brief'] so the operator can
  immediately progress an editorial angle into an article or onto
  myCanvas instead of dead-ending on brief/google-doc.
- LLM-generated specialist responses now have 'article' and
  'mycanvas-remix' in the allowed artifact set documented in the
  schema prompt.
- composeKindForSuggestedArtifact returns null for myCanvas-remix
  types (they don't fit the composer model). The handler in
  handleUseSuggestedArtifact branches: mycanvas-remix navigates to
  the metaMe myCanvas surface with a ?remix= payload carrying the
  specialist response; everything else opens the composer as before.
- WelcomeRightPane's inline SpecialistResponseCard render now
  receives onCreateArtifact, threaded from the existing
  onUseSuggestedArtifact prop, so the chips are clickable on the
  Stack surface (not just inside the dedicated SpecialistsLayout).
  Previously the chips on the default stack surface looked
  interactive but did nothing — the operator had to switch to the
  SpecialistsLayout to actually progress an artifact.
```

## Body

#1 myWorkbench tab registered as a sub-tab of myCanvas. Stub mirrors
MyCanvasTab affordances until the operator confirms the distinction
between 'publish-ready output' (canvas) and 'work-in-progress'
(workbench). Listed in TabRenderer; slug 'my-workbench'; same
activation gate as myCanvas.

#5 + #6 Specialist response actions:
- Quill's editorial template now defaults its suggestedArtifacts to
  ['article', 'mycanvas-remix', 'brief'] so the operator can
  immediately progress an editorial angle into an article or onto
  myCanvas instead of dead-ending on brief/google-doc.
- LLM-generated specialist responses now have 'article' and
  'mycanvas-remix' in the allowed artifact set documented in the
  schema prompt.
- composeKindForSuggestedArtifact returns null for myCanvas-remix
  types (they don't fit the composer model). The handler in
  handleUseSuggestedArtifact branches: mycanvas-remix navigates to
  the metaMe myCanvas surface with a ?remix= payload carrying the
  specialist response; everything else opens the composer as before.
- WelcomeRightPane's inline SpecialistResponseCard render now
  receives onCreateArtifact, threaded from the existing
  onUseSuggestedArtifact prop, so the chips are clickable on the
  Stack surface (not just inside the dedicated SpecialistsLayout).
  Previously the chips on the default stack surface looked
  interactive but did nothing — the operator had to switch to the
  SpecialistsLayout to actually progress an artifact.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/TabRenderer.tsx` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Added | `app/triad/components/codex/tabs/MyWorkbenchTab.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |
| Modified | `data/codex-configs.ts` |
| Modified | `services/agents/specialistRouter.ts` |

## Stats

 6 files changed, 111 insertions(+), 2 deletions(-)
