# Commit Brief: `a3c0eff` — welcome badge: read canonical T1 surface, remove from right-pane carousel

| Field | Value |
|-------|-------|
| SHA | [`a3c0eff`](https://github.com/Kn0w-1/AigentZBeta/commit/a3c0eff6d286ceea6f5390f8e8e1bd93f4e75d8f) |
| Author | Claude |
| Date | 2026-05-22T15:17:24Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
welcome badge: read canonical T1 surface, remove from right-pane carousel

The cartridge-header Welcome badge was reading from useCartridgePersonaGuard
which falls back to id.slice(0,8)+'…' (a truncated UUID) when the
personaDisplayNames registry hasn't populated. Switch it to read from
useActivePersona().surface — displayLabel first, then ownFioHandle,
otherwise hide. This is the same canonical T1 source the wallet uses
and matches the FIO handle the user expects ('info2knyt' etc).

Remove the old 'Welcome, <displayLabel>' line from WelcomeRightPane so
the right-pane identity carousel is now reserved for operational badges
only (ExperienceQube, PersonalGuide, PersonaQube, stage progression).
The Sparkles icon + sticky-left gradient backdrop go with it — the
operational chips render naturally without that scaffolding.
```

## Body

The cartridge-header Welcome badge was reading from useCartridgePersonaGuard
which falls back to id.slice(0,8)+'…' (a truncated UUID) when the
personaDisplayNames registry hasn't populated. Switch it to read from
useActivePersona().surface — displayLabel first, then ownFioHandle,
otherwise hide. This is the same canonical T1 source the wallet uses
and matches the FIO handle the user expects ('info2knyt' etc).

Remove the old 'Welcome, <displayLabel>' line from WelcomeRightPane so
the right-pane identity carousel is now reserved for operational badges
only (ExperienceQube, PersonalGuide, PersonaQube, stage progression).
The Sparkles icon + sticky-left gradient backdrop go with it — the
operational chips render naturally without that scaffolding.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/CodexPanelDynamic.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |

## Stats

 2 files changed, 24 insertions(+), 18 deletions(-)
