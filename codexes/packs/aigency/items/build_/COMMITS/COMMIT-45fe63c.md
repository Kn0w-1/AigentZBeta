# Commit Brief: `45fe63c` — aigentMe copilot: ground left narrative in right-pane shape + tooltip maturity buttons

| Field | Value |
|-------|-------|
| SHA | [`45fe63c`](https://github.com/Kn0w-1/AigentZBeta/commit/45fe63c2c4a8680a5d6adbefd4fb7571efa264f2) |
| Author | Claude |
| Date | 2026-05-26T16:19:51Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
aigentMe copilot: ground left narrative in right-pane shape + tooltip maturity buttons

Phase A of the refinements pass.

Fixes the regression where the left chat emitted placeholder strings
('[Priority 1]', '[Action 1]', '[Event/Document/Message 1]') instead of
narrating the actual brief / NBA rows on the right. Root cause: the
chat POST carried no ground truth, so the LLM invented a generic
template. CopilotKit readables don't apply — /api/codex/chat is the
custom route, not CopilotKit's own client.

- SmartTriadCopilotLayer: add a groundContext prop, mirror it onto a
  ref so handleSend always reads the freshest snapshot (chip clicks
  fire a parallel right-pane fetch; the ref captures the value at
  POST time, not at chip-click time). Forward groundContext +
  personaId on every chat POST.
- AigentMeWelcomeSplitTab: build a T1-safe groundContext snapshot
  from brief + moveForwardResult + expModel + activeCartridges +
  pendingApproval + queuedIntentIds (including the Move D
  nbaPromptHints per row) and pass it to the copilot.
- /api/codex/chat: read body.groundContext, plumb onto userContext,
  and append a 'Right-pane ground truth' block to the aigent-me
  system prompt with explicit instruction to narrate the rows
  by label + rationale and never emit '[Priority N]' placeholders.

Phase A.2 — tooltips on the PersonalGuide wizard maturity buttons so
operators understand 'Noticing → Stewarding' without guessing.

- types/experienceGuide.ts: add MATURITY_DESCRIPTION + SPHERE_DESCRIPTION
  maps.
- PersonalGuideSetupWizard: render sphere description under each step
  header, add title + aria-label tooltips on each maturity button,
  and a short hover hint.
```

## Body

Phase A of the refinements pass.

Fixes the regression where the left chat emitted placeholder strings
('[Priority 1]', '[Action 1]', '[Event/Document/Message 1]') instead of
narrating the actual brief / NBA rows on the right. Root cause: the
chat POST carried no ground truth, so the LLM invented a generic
template. CopilotKit readables don't apply — /api/codex/chat is the
custom route, not CopilotKit's own client.

- SmartTriadCopilotLayer: add a groundContext prop, mirror it onto a
  ref so handleSend always reads the freshest snapshot (chip clicks
  fire a parallel right-pane fetch; the ref captures the value at
  POST time, not at chip-click time). Forward groundContext +
  personaId on every chat POST.
- AigentMeWelcomeSplitTab: build a T1-safe groundContext snapshot
  from brief + moveForwardResult + expModel + activeCartridges +
  pendingApproval + queuedIntentIds (including the Move D
  nbaPromptHints per row) and pass it to the copilot.
- /api/codex/chat: read body.groundContext, plumb onto userContext,
  and append a 'Right-pane ground truth' block to the aigent-me
  system prompt with explicit instruction to narrate the rows
  by label + rationale and never emit '[Priority N]' placeholders.

Phase A.2 — tooltips on the PersonalGuide wizard maturity buttons so
operators understand 'Noticing → Stewarding' without guessing.

- types/experienceGuide.ts: add MATURITY_DESCRIPTION + SPHERE_DESCRIPTION
  maps.
- PersonalGuideSetupWizard: render sphere description under each step
  header, add title + aria-label tooltips on each maturity button,
  and a short hover hint.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/codex/chat/route.ts` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/setup/PersonalGuideSetupWizard.tsx` |
| Modified | `components/smarttriad/copilot/SmartTriadCopilotLayer.tsx` |
| Modified | `types/experienceGuide.ts` |

## Stats

 5 files changed, 258 insertions(+), 3 deletions(-)
