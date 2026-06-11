# Commit Brief: `57550ec` — compose-modal generation: restore alpha prompt path for composer artifacts

| Field | Value |
|-------|-------|
| SHA | [`57550ec`](https://github.com/Kn0w-1/AigentZBeta/commit/57550ec4f6b0e7196baa52e1ae226fc3504bc30d) |
| Author | Claude |
| Date | 2026-05-26T21:05:40Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
compose-modal generation: restore alpha prompt path for composer artifacts

Operator: 'I still don't have these being generated in the green
collective auto layout modal. Not even for generating a doc which is
a regression somewhere... Can you track down the regression and then
restore and mimic the pattern across the other act modals
accordingly?'

Root cause: Phase F.1's unified dispatcher routed ALL composer-class
artifacts through dispatchArtifact's buildPromptFromPayload step.
That helper produced a thin markdown-headed prompt (just a # title +
summary block) which the doc-draft endpoint couldn't draft from
usefully — the modal opened but the form never populated. The alpha
flow that worked used buildPromptForSuggestedArtifact (directive
style: 'Draft a <kind> that operationalises <specialist>'s
recommendation: <title>. Context: <summary>. Key points to cover:
<bullets>...') which produces a much richer draft.

Fix — restore the alpha path on the composer leg of BOTH entry points
while keeping the new dispatcher for genuinely new destinations:

NBE Act post-approval (handleApprovalApprove):
  - composer-class artifacts (gmail-draft, google-doc, calendar-block,
    slide-outline, brief, venture-report, and legacy
    metame.use-workspace-*) restore the alpha path verbatim:
      setComposerInitialPrompt(handoffHint || null);
      setComposerKind(<kind>);
      setActiveLayoutId('composer');
    Modal's existing auto-fire useEffect handles draftWithPrompt when
    handoffHint is present (richer rerank-emitted prompt). When
    handoffHint is null the modal opens empty — same as alpha; user
    types and clicks Draft.
  - Non-composer destinations (canvas / workbench / studio /
    marketa-campaign) still go through dispatchArtifact since those
    are new paths and not affected by the regression.

Specialist chips (handleUseSuggestedArtifact):
  - composer-class artifacts now call buildPromptForSuggestedArtifact
    (the alpha helper) and use the alpha setters directly. Draft
    quality matches what the SpecialistsLayout flow has been producing
    end-to-end since alpha.
  - Non-composer destinations still go through dispatchArtifact.

Net effect: composer modals now generate content again across the
board (Email / Event / Doc / Sheet / Slides / Marketa) — same code
path the alpha relied on. The unified dispatcher remains the SoT for
canvas / workbench / studio / marketa-campaign — those new paths
needed a centralised router and don't share the auto-fire concern.
```

## Body

Operator: 'I still don't have these being generated in the green
collective auto layout modal. Not even for generating a doc which is
a regression somewhere... Can you track down the regression and then
restore and mimic the pattern across the other act modals
accordingly?'

Root cause: Phase F.1's unified dispatcher routed ALL composer-class
artifacts through dispatchArtifact's buildPromptFromPayload step.
That helper produced a thin markdown-headed prompt (just a # title +
summary block) which the doc-draft endpoint couldn't draft from
usefully — the modal opened but the form never populated. The alpha
flow that worked used buildPromptForSuggestedArtifact (directive
style: 'Draft a <kind> that operationalises <specialist>'s
recommendation: <title>. Context: <summary>. Key points to cover:
<bullets>...') which produces a much richer draft.

Fix — restore the alpha path on the composer leg of BOTH entry points
while keeping the new dispatcher for genuinely new destinations:

NBE Act post-approval (handleApprovalApprove):
  - composer-class artifacts (gmail-draft, google-doc, calendar-block,
    slide-outline, brief, venture-report, and legacy
    metame.use-workspace-*) restore the alpha path verbatim:
      setComposerInitialPrompt(handoffHint || null);
      setComposerKind(<kind>);
      setActiveLayoutId('composer');
    Modal's existing auto-fire useEffect handles draftWithPrompt when
    handoffHint is present (richer rerank-emitted prompt). When
    handoffHint is null the modal opens empty — same as alpha; user
    types and clicks Draft.
  - Non-composer destinations (canvas / workbench / studio /
    marketa-campaign) still go through dispatchArtifact since those
    are new paths and not affected by the regression.

Specialist chips (handleUseSuggestedArtifact):
  - composer-class artifacts now call buildPromptForSuggestedArtifact
    (the alpha helper) and use the alpha setters directly. Draft
    quality matches what the SpecialistsLayout flow has been producing
    end-to-end since alpha.
  - Non-composer destinations still go through dispatchArtifact.

Net effect: composer modals now generate content again across the
board (Email / Event / Doc / Sheet / Slides / Marketa) — same code
path the alpha relied on. The unified dispatcher remains the SoT for
canvas / workbench / studio / marketa-campaign — those new paths
needed a centralised router and don't share the auto-fire concern.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |

## Stats

 1 file changed, 46 insertions(+), 34 deletions(-)
