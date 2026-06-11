# Commit Brief: `ed2f4fe` — artifact progression: class-wide unified dispatcher across specialists + NBE Act

| Field | Value |
|-------|-------|
| SHA | [`ed2f4fe`](https://github.com/Kn0w-1/AigentZBeta/commit/ed2f4fed5f4a9cf653619d449673a0220011430f) |
| Author | Claude |
| Date | 2026-05-26T20:23:44Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
artifact progression: class-wide unified dispatcher across specialists + NBE Act

Operator feedback: 'this needs to be applied across all 6 or so action
templates ... we don't have to find them and go through them piece
meal — which includes all the google work actions and I think I saw
one even for generating a video using a skill from the studio. This
is exactly the kind of thing we want to be able to do but we want
this enabled on a class basis.'

Single SoT now: classifySuggestedArtifact() + dispatchArtifact().
Every surface that wants to progress an artifact (specialist chip /
NBE Act post-approval / future create-doc chips / chat-emitted tool
calls) routes through the same classifier so each artifact lands
predictably:

  composer surfaces (gmail/event/doc/sheet/slides/marketa)
    → compose modal with inferred draft prompt
  partner-brief / myworkbench-draft
    → myWorkbench (private internal artifacts)
  mycanvas-remix / article (sometimes)
    → myCanvas (public publishing to KNYT Pulse / Qriptopian Pulse)
  image-prompt / video-script / post-set
    → metaMe Studio (skill invocation)
  marketa-campaign
    → marketa composer

Files:
- services/agents/specialistRouter.ts: every specialist template
  (marketa, quill, kn0w1, aigent-z, aigent-c, aigent-nakamoto,
  moneypenny, metaye) now carries role-appropriate suggestedArtifacts
  by default. LLM-side schema prompt expanded to document the full
  artifact catalog + per-type routing rule so generated responses
  pick from the same set as templates.
- app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx:
  classifySuggestedArtifact() returns a tagged union (composer |
  mycanvas-remix | myworkbench-draft | studio | marketa-campaign |
  partner-brief | unknown). dispatchArtifact() is the single
  navigator that handles each kind. handleUseSuggestedArtifact and
  the NBE Act post-approval path both funnel through dispatchArtifact
  — eliminating the prior fork where chips and Act buttons used
  different routers.
- composeKindForAction / composeKindForSuggestedArtifact retained
  as back-compat thin wrappers around the new classifier. Legacy
  metame.use-workspace-* ids still route correctly via the wrapper
  fallback inside the approval handler.

Compose strip (Email/Event/Doc/Sheet/Slides/Marketa) intentionally
unchanged — explicit user intent. Surface still useful when the user
wants to pick the destination directly rather than letting the
artifact-type classifier decide.
```

## Body

Operator feedback: 'this needs to be applied across all 6 or so action
templates ... we don't have to find them and go through them piece
meal — which includes all the google work actions and I think I saw
one even for generating a video using a skill from the studio. This
is exactly the kind of thing we want to be able to do but we want
this enabled on a class basis.'

Single SoT now: classifySuggestedArtifact() + dispatchArtifact().
Every surface that wants to progress an artifact (specialist chip /
NBE Act post-approval / future create-doc chips / chat-emitted tool
calls) routes through the same classifier so each artifact lands
predictably:

  composer surfaces (gmail/event/doc/sheet/slides/marketa)
    → compose modal with inferred draft prompt
  partner-brief / myworkbench-draft
    → myWorkbench (private internal artifacts)
  mycanvas-remix / article (sometimes)
    → myCanvas (public publishing to KNYT Pulse / Qriptopian Pulse)
  image-prompt / video-script / post-set
    → metaMe Studio (skill invocation)
  marketa-campaign
    → marketa composer

Files:
- services/agents/specialistRouter.ts: every specialist template
  (marketa, quill, kn0w1, aigent-z, aigent-c, aigent-nakamoto,
  moneypenny, metaye) now carries role-appropriate suggestedArtifacts
  by default. LLM-side schema prompt expanded to document the full
  artifact catalog + per-type routing rule so generated responses
  pick from the same set as templates.
- app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx:
  classifySuggestedArtifact() returns a tagged union (composer |
  mycanvas-remix | myworkbench-draft | studio | marketa-campaign |
  partner-brief | unknown). dispatchArtifact() is the single
  navigator that handles each kind. handleUseSuggestedArtifact and
  the NBE Act post-approval path both funnel through dispatchArtifact
  — eliminating the prior fork where chips and Act buttons used
  different routers.
- composeKindForAction / composeKindForSuggestedArtifact retained
  as back-compat thin wrappers around the new classifier. Legacy
  metame.use-workspace-* ids still route correctly via the wrapper
  fallback inside the approval handler.

Compose strip (Email/Event/Doc/Sheet/Slides/Marketa) intentionally
unchanged — explicit user intent. Surface still useful when the user
wants to pick the destination directly rather than letting the
artifact-type classifier decide.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `services/agents/specialistRouter.ts` |

## Stats

 2 files changed, 311 insertions(+), 75 deletions(-)
