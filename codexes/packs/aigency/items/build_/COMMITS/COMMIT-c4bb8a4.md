# Commit Brief: `c4bb8a4` — SpecialistsLayout: visual refinements + render fix for in-flight + reply

| Field | Value |
|-------|-------|
| SHA | [`c4bb8a4`](https://github.com/Kn0w-1/AigentZBeta/commit/c4bb8a4bc472e063e522bad537df550bafaeaa33) |
| Author | Claude |
| Date | 2026-05-24T18:26:55Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
SpecialistsLayout: visual refinements + render fix for in-flight + reply

Visual:
- Recommendation card collapses from ~4 rows to 2: eyebrow + action
  buttons (Consult / alternates) sit on the same row right-justified,
  specialist name + reason render inline on row 2 with the reason
  truncated and full text in tooltip.
- Roster chips compress to max 3 rows by promoting the "Needs
  activation" badge into a small "Locked" pill inline with the
  specialist label on row 1, freeing rows 2-3 for the description.
  The full activation label is in the title tooltip.
- Active-specialist focus card switches from violet to emerald so the
  selected agent reads visually distinct from the violet "aigentMe
  suggests" recommendation banner sitting above it. Locked
  specialists still surface amber so the gate reads urgent.
- Left/right columns rebalance to 50/50 (was 55/45). The metaVatar
  rendering layer reads the copilot's getBoundingClientRect via the
  --metaavatar-copilot-w CSS variable, so it rescales automatically
  when the copilot container narrows — no separate change required.

Activations deep-link:
- "Open Activations" on the locked-specialist focus card now uses the
  canonical CartridgePresenceRegistry.tryOpenInMountedCartridge() to
  switch the parent cartridge to its 'activations' top-nav tab,
  instead of expanding the Experience accordion inside the stack
  layout. Falls back to the prior expand-accordion behavior when the
  registry has no mounted cartridge (mount race / standalone embed).

Render fix — prompt + inference were not visible after Send:
- The reply section was below the composer in the body, so the
  response landed below the fold of the right-pane scroll container
  and the operator never saw it. Reorders to:
    1. Recommendation
    2. Roster
    3. Active specialist focus
    4. Reply (or in-flight "Asking <specialist>…" placeholder)
    5. Composer
    6. Prior consultations
  Adds an AskingPlaceholder card that lands the instant the composer
  fires so the operator gets immediate feedback; replaced by the real
  SpecialistResponseCard the moment the request resolves. Reply
  heading also switches to emerald to align with the active-agent
  accent.
```

## Body

Visual:
- Recommendation card collapses from ~4 rows to 2: eyebrow + action
  buttons (Consult / alternates) sit on the same row right-justified,
  specialist name + reason render inline on row 2 with the reason
  truncated and full text in tooltip.
- Roster chips compress to max 3 rows by promoting the "Needs
  activation" badge into a small "Locked" pill inline with the
  specialist label on row 1, freeing rows 2-3 for the description.
  The full activation label is in the title tooltip.
- Active-specialist focus card switches from violet to emerald so the
  selected agent reads visually distinct from the violet "aigentMe
  suggests" recommendation banner sitting above it. Locked
  specialists still surface amber so the gate reads urgent.
- Left/right columns rebalance to 50/50 (was 55/45). The metaVatar
  rendering layer reads the copilot's getBoundingClientRect via the
  --metaavatar-copilot-w CSS variable, so it rescales automatically
  when the copilot container narrows — no separate change required.

Activations deep-link:
- "Open Activations" on the locked-specialist focus card now uses the
  canonical CartridgePresenceRegistry.tryOpenInMountedCartridge() to
  switch the parent cartridge to its 'activations' top-nav tab,
  instead of expanding the Experience accordion inside the stack
  layout. Falls back to the prior expand-accordion behavior when the
  registry has no mounted cartridge (mount race / standalone embed).

Render fix — prompt + inference were not visible after Send:
- The reply section was below the composer in the body, so the
  response landed below the fold of the right-pane scroll container
  and the operator never saw it. Reorders to:
    1. Recommendation
    2. Roster
    3. Active specialist focus
    4. Reply (or in-flight "Asking <specialist>…" placeholder)
    5. Composer
    6. Prior consultations
  Adds an AskingPlaceholder card that lands the instant the composer
  fires so the operator gets immediate feedback; replaced by the real
  SpecialistResponseCard the moment the request resolves. Reply
  heading also switches to emerald to align with the active-agent
  accent.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/welcome/layouts/SpecialistsLayout.tsx` |

## Stats

 2 files changed, 163 insertions(+), 78 deletions(-)
