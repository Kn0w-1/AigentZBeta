# Commit Brief: `029598c` — act button: directive fallback prompt when rerank doesn't emit a hint

| Field | Value |
|-------|-------|
| SHA | [`029598c`](https://github.com/Kn0w-1/AigentZBeta/commit/029598cb60a45544ca87b0e4fa7bfee1b9832099) |
| Author | Claude |
| Date | 2026-05-26T21:39:54Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
act button: directive fallback prompt when rerank doesn't emit a hint

Operator: 'the act button is no longer auto populating the
generate/composer modal. None of them are. this part of the workflow
broke.'

Root cause: post-approve composer dispatch only seeded
composerInitialPrompt when handoffHint (from brief.nbaPromptHints /
moveForwardResult.nbaPromptHints) was non-empty. When the LLM rerank
didn't emit a hint for the action — which is the dominant case in
alpha — composerInitialPrompt landed null, the modal's auto-fire
useEffect bailed (`if (!initialPrompt) return`), and the operator
saw an empty form. The autopopulate was 'restored' in 57550ec4 but
only for the rerank-hint-present path.

Fix: new buildPromptForNbeAction() builder mirrors the directive
shape buildPromptForSuggestedArtifact emits for specialist chips —
'Draft a <kind> that operationalises this next-best action:
<label>. Context: <rationale>. Cartridge: <cartridge>. ...'. The
post-approve flow now falls back to this when handoffHint is null,
guaranteeing every composer-class Act lands the operator on a
populated form.

Behaviour:
  - handoffHint present (LLM rerank emitted nbaPromptHints[id])
    → composer auto-fires with the richer hint
  - handoffHint absent
    → composer auto-fires with the directive fallback built from
      action.label + action.rationale + action.cartridge +
      action.suggestedArtifact

Either way the modal arrives populated and the draft fires.
```

## Body

Operator: 'the act button is no longer auto populating the
generate/composer modal. None of them are. this part of the workflow
broke.'

Root cause: post-approve composer dispatch only seeded
composerInitialPrompt when handoffHint (from brief.nbaPromptHints /
moveForwardResult.nbaPromptHints) was non-empty. When the LLM rerank
didn't emit a hint for the action — which is the dominant case in
alpha — composerInitialPrompt landed null, the modal's auto-fire
useEffect bailed (`if (!initialPrompt) return`), and the operator
saw an empty form. The autopopulate was 'restored' in 57550ec4 but
only for the rerank-hint-present path.

Fix: new buildPromptForNbeAction() builder mirrors the directive
shape buildPromptForSuggestedArtifact emits for specialist chips —
'Draft a <kind> that operationalises this next-best action:
<label>. Context: <rationale>. Cartridge: <cartridge>. ...'. The
post-approve flow now falls back to this when handoffHint is null,
guaranteeing every composer-class Act lands the operator on a
populated form.

Behaviour:
  - handoffHint present (LLM rerank emitted nbaPromptHints[id])
    → composer auto-fires with the richer hint
  - handoffHint absent
    → composer auto-fires with the directive fallback built from
      action.label + action.rationale + action.cartridge +
      action.suggestedArtifact

Either way the modal arrives populated and the draft fires.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |

## Stats

 1 file changed, 41 insertions(+), 4 deletions(-)
