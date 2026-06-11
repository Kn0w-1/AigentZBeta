# Commit Brief: `e7d7974` — Ask Specialists chip: mount SpecialistsLayout (parity with Brief/Move-forward/Venture chips)

| Field | Value |
|-------|-------|
| SHA | [`e7d7974`](https://github.com/Kn0w-1/AigentZBeta/commit/e7d797424f27a3d360ad5fd5c9bc5113ab0ad071) |
| Author | Claude |
| Date | 2026-05-28T15:04:17Z |
| Branch | dev (direct push) |
| Type | `refactor` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
Ask Specialists chip: mount SpecialistsLayout (parity with Brief/Move-forward/Venture chips)

The Ask Specialists left-pane chip was missing the setActiveLayoutId call
that the other three Capsule chips fire. Clicking it engaged the
capsule but left activeLayoutId at 'stack', so the operator landed on
the WelcomeRightPane manual-fallback surface — specialist responses and
their suggested-artifact CTAs rendered there instead of inside the
Ask Specialists Capsule.
```

## Body

The Ask Specialists left-pane chip was missing the setActiveLayoutId call
that the other three Capsule chips fire. Clicking it engaged the
capsule but left activeLayoutId at 'stack', so the operator landed on
the WelcomeRightPane manual-fallback surface — specialist responses and
their suggested-artifact CTAs rendered there instead of inside the
Ask Specialists Capsule.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |

## Stats

 1 file changed, 1 insertion(+)
