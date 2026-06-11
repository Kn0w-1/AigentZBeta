# Commit Brief: `bdb61ee` — aigentMe welcome refinements: tighter specialists, MoneyPenny + Metayé, Google Sheets, collapsibles

| Field | Value |
|-------|-------|
| SHA | [`bdb61ee`](https://github.com/Kn0w-1/AigentZBeta/commit/bdb61ee5b7aef85e115ad4a8fb72276838a25266) |
| Author | Claude |
| Date | 2026-05-13T16:44:54Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
aigentMe welcome refinements: tighter specialists, MoneyPenny + Metayé, Google Sheets, collapsibles

Specialists (clean up + grow to eight):
- Shorten labels: 'Quill, editor of The Qriptopian, powered by Aigent Q' →
  'Quill'; 'Aigent Nakamoto (Satoshi)' → 'Nakamoto'. Description lines
  trimmed for Kn0w1 (KNYT world, PCS, knowledge economics, missions) and
  Nakamoto (Decentralisation, Qripto protocols, ecosystem policy).
- Add MoneyPenny (Q¢ economics, micro-transactions, payment ops) and
  Metayé (Sovereign Cybernetic Polity, governance, civic primitives).
  Both wired across personas.ts, specialistRouter (SpecialistId union,
  persona keys, labels, request-type inference, template fallback) and
  bootstrap route.

Ask integrated into specialist cards:
- Drop the ask-marketa / ask-quill / ask-kn0w1 / ask-nakamoto / ask-aigent-z
  / ask-aigent-c CTAs from the primary action grid (bootstrap + welcome
  CTA filter belt-and-braces).
- Each specialist card now carries an inline Ask button with a
  per-specialist expander (textarea + send + cancel) that posts to
  /api/assistant/ask-agent. Response renders inline beneath the card.
  Preview specialists get a 'soon' badge instead.

Google Sheets — fully wired end-to-end:
- services/google/oauth.ts: GoogleSource 'sheets' + scopes.
- services/google/connectors.ts: google.sheets.create connector with
  header + rows seeding via spreadsheets.values:append.
- services/agents/draftGoogleSheet.ts: LLM-or-template drafter that
  returns title + sheetName + rows[][] with header.
- app/api/assistant/draft-sheet/route.ts: spine-auth POST mirroring
  draft-doc.
- app/api/assistant/create-artifact/route.ts: google-sheet artifact
  type and drive-destination branch.
- components/metame/connections/ComposeGoogleSheetModal.tsx: chief-of-
  staff compose modal with drafter strip, header row editor, expandable
  data rows.
- components/metame/connections/GoogleConnectionsPanel.tsx: Sheets card.
- AigentMeWelcomeTab: Sheet button added to the compose strip; full
  prop / handler / modal wiring.

Collapsible below-the-fold sections:
- New CollapsibleSection primitive colocated in AigentMeWelcomeTab.
- Specialists, 'Open a cartridge' (QuickLinks), Google Workspace, and
  Active context are now collapsible (default closed) with a one-line
  summary visible when closed. ExperienceModel card stays visible —
  it's the strategy anchor.
```

## Body

Specialists (clean up + grow to eight):
- Shorten labels: 'Quill, editor of The Qriptopian, powered by Aigent Q' →
  'Quill'; 'Aigent Nakamoto (Satoshi)' → 'Nakamoto'. Description lines
  trimmed for Kn0w1 (KNYT world, PCS, knowledge economics, missions) and
  Nakamoto (Decentralisation, Qripto protocols, ecosystem policy).
- Add MoneyPenny (Q¢ economics, micro-transactions, payment ops) and
  Metayé (Sovereign Cybernetic Polity, governance, civic primitives).
  Both wired across personas.ts, specialistRouter (SpecialistId union,
  persona keys, labels, request-type inference, template fallback) and
  bootstrap route.

Ask integrated into specialist cards:
- Drop the ask-marketa / ask-quill / ask-kn0w1 / ask-nakamoto / ask-aigent-z
  / ask-aigent-c CTAs from the primary action grid (bootstrap + welcome
  CTA filter belt-and-braces).
- Each specialist card now carries an inline Ask button with a
  per-specialist expander (textarea + send + cancel) that posts to
  /api/assistant/ask-agent. Response renders inline beneath the card.
  Preview specialists get a 'soon' badge instead.

Google Sheets — fully wired end-to-end:
- services/google/oauth.ts: GoogleSource 'sheets' + scopes.
- services/google/connectors.ts: google.sheets.create connector with
  header + rows seeding via spreadsheets.values:append.
- services/agents/draftGoogleSheet.ts: LLM-or-template drafter that
  returns title + sheetName + rows[][] with header.
- app/api/assistant/draft-sheet/route.ts: spine-auth POST mirroring
  draft-doc.
- app/api/assistant/create-artifact/route.ts: google-sheet artifact
  type and drive-destination branch.
- components/metame/connections/ComposeGoogleSheetModal.tsx: chief-of-
  staff compose modal with drafter strip, header row editor, expandable
  data rows.
- components/metame/connections/GoogleConnectionsPanel.tsx: Sheets card.
- AigentMeWelcomeTab: Sheet button added to the compose strip; full
  prop / handler / modal wiring.

Collapsible below-the-fold sections:
- New CollapsibleSection primitive colocated in AigentMeWelcomeTab.
- Specialists, 'Open a cartridge' (QuickLinks), Google Workspace, and
  Active context are now collapsible (default closed) with a one-line
  summary visible when closed. ExperienceModel card stays visible —
  it's the strategy anchor.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/assistant/bootstrap/route.ts` |
| Modified | `app/api/assistant/create-artifact/route.ts` |
| Added | `app/api/assistant/draft-sheet/route.ts` |
| Modified | `app/data/personas.ts` |
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeTab.tsx` |
| Added | `components/metame/connections/ComposeGoogleSheetModal.tsx` |
| Modified | `components/metame/connections/GoogleConnectionsPanel.tsx` |
| Added | `services/agents/draftGoogleSheet.ts` |
| Modified | `services/agents/specialistRouter.ts` |
| Modified | `services/google/connectors.ts` |
| Modified | `services/google/oauth.ts` |

## Stats

 12 files changed, 1170 insertions(+), 74 deletions(-)
