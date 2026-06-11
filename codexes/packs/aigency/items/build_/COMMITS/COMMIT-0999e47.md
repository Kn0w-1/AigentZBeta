# Commit Brief: `0999e47` — nbe: emit nbaPromptHints from rerank + render as aigentMe's take + seed composer on Act

| Field | Value |
|-------|-------|
| SHA | [`0999e47`](https://github.com/Kn0w-1/AigentZBeta/commit/0999e47a0beaee40188c2196e03eb615134be2c8) |
| Author | Claude |
| Date | 2026-05-26T15:11:48Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
nbe: emit nbaPromptHints from rerank + render as aigentMe's take + seed composer on Act

Move D — left/right contextual NBA pre-population.

- nbeLlmRerank: extend SYSTEM_PROMPT to require per-NBA prompt hint
  emission alongside topReason; parse + validate against the eligible
  id set; clamp each hint to <=200 chars.
- briefBuilder: thread nbaPromptHints through BriefShape and
  MoveForwardShape so the brief and move-forward routes return them.
- NextBestActionCard: accept promptHint prop and render an italic
  'aigentMe's take' line under the rationale.
- BriefCard + WelcomeRightPane: pass per-NBA hints down to each card.
- AigentMeWelcomeSplitTab: capture the hint on Act, stash through
  approval, and seed composerInitialPrompt when the post-approval
  hand-off opens a compose modal — so the inline form lands populated
  with the LLM's framing instead of blank.
```

## Body

Move D — left/right contextual NBA pre-population.

- nbeLlmRerank: extend SYSTEM_PROMPT to require per-NBA prompt hint
  emission alongside topReason; parse + validate against the eligible
  id set; clamp each hint to <=200 chars.
- briefBuilder: thread nbaPromptHints through BriefShape and
  MoveForwardShape so the brief and move-forward routes return them.
- NextBestActionCard: accept promptHint prop and render an italic
  'aigentMe's take' line under the rationale.
- BriefCard + WelcomeRightPane: pass per-NBA hints down to each card.
- AigentMeWelcomeSplitTab: capture the hint on Act, stash through
  approval, and seed composerInitialPrompt when the post-approval
  hand-off opens a compose modal — so the inline form lands populated
  with the LLM's framing instead of blank.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/cards/BriefCard.tsx` |
| Modified | `components/metame/cards/NextBestActionCard.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |
| Modified | `services/orchestration/briefBuilder.ts` |
| Modified | `services/orchestration/nbeLlmRerank.ts` |

## Stats

 6 files changed, 117 insertions(+), 12 deletions(-)
