# Commit Brief: `f21e904` — request access chip: contextual surfacing only (no default render)

| Field | Value |
|-------|-------|
| SHA | [`f21e904`](https://github.com/Kn0w-1/AigentZBeta/commit/f21e90493acf46139c8162784f75aa220facf453) |
| Author | Claude |
| Date | 2026-05-26T20:26:18Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
request access chip: contextual surfacing only (no default render)

Operator feedback: 'don't render this by default. It should be
contextual. If a user wants to launch an activity that requires a
capability in another cartridge — e.g. monitor more than one venture
— they should be recommended upgrading to that cartridge.'

The alpha behaviour rendered the pulse chip any time the persona had
no admin grants. That's noisy — most users don't need a 'request
access' nudge when they're happily working in their current
cartridges. They need one only when they're about to do something
they can't.

New behaviour:

- RequestAccessChip now requires recommendedCartridgeSlug. When null
  the chip renders nothing. The render gate is now CONTEXTUAL, not
  absence-of-grants.
- AigentMeWelcomeSplitTab computes the recommendation by scanning
  brief.nextBestActions + moveForwardResult.alternates for any NBA
  whose target cartridge ISN'T in the persona's active set. First
  miss becomes the recommendation. Returns null when every NBA's
  cartridge is already active. Always-on cartridges (metame,
  agentiq-os) are excluded from the candidate list — they don't
  require alpha access.
- Chip label is now cartridge-specific: 'Request KNYT access',
  'Request metaMe Venture Lab access', etc. Removes the prior
  ambiguity ('why am I requesting access to metaMe?').
- Dismiss persistence is scoped per cartridge slug — dismissing
  'Venture Lab' for the session doesn't hide a later 'Marketa' nudge
  when the operator's brief flips to a Marketa-cartridge NBA.

Wiring: WelcomeRightPane gains recommendedAccessCartridgeSlug +
recommendedAccessCartridgeLabel props; AigentMeWelcomeSplitTab
populates them via a new useMemo that walks the loaded NBAs.
```

## Body

Operator feedback: 'don't render this by default. It should be
contextual. If a user wants to launch an activity that requires a
capability in another cartridge — e.g. monitor more than one venture
— they should be recommended upgrading to that cartridge.'

The alpha behaviour rendered the pulse chip any time the persona had
no admin grants. That's noisy — most users don't need a 'request
access' nudge when they're happily working in their current
cartridges. They need one only when they're about to do something
they can't.

New behaviour:

- RequestAccessChip now requires recommendedCartridgeSlug. When null
  the chip renders nothing. The render gate is now CONTEXTUAL, not
  absence-of-grants.
- AigentMeWelcomeSplitTab computes the recommendation by scanning
  brief.nextBestActions + moveForwardResult.alternates for any NBA
  whose target cartridge ISN'T in the persona's active set. First
  miss becomes the recommendation. Returns null when every NBA's
  cartridge is already active. Always-on cartridges (metame,
  agentiq-os) are excluded from the candidate list — they don't
  require alpha access.
- Chip label is now cartridge-specific: 'Request KNYT access',
  'Request metaMe Venture Lab access', etc. Removes the prior
  ambiguity ('why am I requesting access to metaMe?').
- Dismiss persistence is scoped per cartridge slug — dismissing
  'Venture Lab' for the session doesn't hide a later 'Marketa' nudge
  when the operator's brief flips to a Marketa-cartridge NBA.

Wiring: WelcomeRightPane gains recommendedAccessCartridgeSlug +
recommendedAccessCartridgeLabel props; AigentMeWelcomeSplitTab
populates them via a new useMemo that walks the loaded NBAs.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |

## Stats

 2 files changed, 137 insertions(+), 55 deletions(-)
