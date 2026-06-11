# Commit Brief: `1f27cea` — myCanvas vs myWorkbench: public-publishing vs private-internal differentiation

| Field | Value |
|-------|-------|
| SHA | [`1f27cea`](https://github.com/Kn0w-1/AigentZBeta/commit/1f27cead4355ee7ff529ac3141853f8a337f37fc) |
| Author | Claude |
| Date | 2026-05-26T20:30:08Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
myCanvas vs myWorkbench: public-publishing vs private-internal differentiation

Operator: 'myCanvas is more specifically for publishing content to
KNYT Pulse or Qriptopian Pulse or runtimes — myWorkbench is more for
producing private confidential work — emails, partner-briefs,
reports, decks etc. It's for more internal and private workloads
and intents.'

MyCanvasTab now accepts a surface prop ('canvas' | 'workbench'):

- 'canvas' (default): visibility defaults to 'invited' (shareable);
  heading reads 'myCanvas · public · publishable'.
- 'workbench': visibility defaults to 'private'; heading reads
  'myWorkbench · private · internal'.

Both surfaces share the same /api/mycanvas/entries backend + same
editor. The differentiation lives in:

  - default visibility on create
  - metaJson.surface stamp on create (filtered client-side on list
    render; server-side filter is a follow-up)
  - URL param consumer (?remix= for canvas, ?draft= for workbench)
    auto-creates an entry seeded from the specialist artifact
    dispatch payload (title + summary + recommendations)
  - createdByPersonaId stamp on seeded entries so the publishing
    surfaces can populate the Creator/byline field correctly when
    the entry goes to KNYT Pulse / Qriptopian Pulse

MyWorkbenchTab is now a one-line wrapper that mounts MyCanvasTab
with surface='workbench'. The two surfaces stay in sync because
they share the source-of-truth code.

The seeded-entry consumer closes the loop on Phase F.1: dispatched
artifacts from any surface (specialist chips, NBE Act, future
chat tool-calls) land as a hydrated entry in the right surface
with the Creator already tagged.
```

## Body

Operator: 'myCanvas is more specifically for publishing content to
KNYT Pulse or Qriptopian Pulse or runtimes — myWorkbench is more for
producing private confidential work — emails, partner-briefs,
reports, decks etc. It's for more internal and private workloads
and intents.'

MyCanvasTab now accepts a surface prop ('canvas' | 'workbench'):

- 'canvas' (default): visibility defaults to 'invited' (shareable);
  heading reads 'myCanvas · public · publishable'.
- 'workbench': visibility defaults to 'private'; heading reads
  'myWorkbench · private · internal'.

Both surfaces share the same /api/mycanvas/entries backend + same
editor. The differentiation lives in:

  - default visibility on create
  - metaJson.surface stamp on create (filtered client-side on list
    render; server-side filter is a follow-up)
  - URL param consumer (?remix= for canvas, ?draft= for workbench)
    auto-creates an entry seeded from the specialist artifact
    dispatch payload (title + summary + recommendations)
  - createdByPersonaId stamp on seeded entries so the publishing
    surfaces can populate the Creator/byline field correctly when
    the entry goes to KNYT Pulse / Qriptopian Pulse

MyWorkbenchTab is now a one-line wrapper that mounts MyCanvasTab
with surface='workbench'. The two surfaces stay in sync because
they share the source-of-truth code.

The seeded-entry consumer closes the loop on Phase F.1: dispatched
artifacts from any surface (specialist chips, NBE Act, future
chat tool-calls) land as a hydrated entry in the right surface
with the Creator already tagged.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |
| Modified | `app/triad/components/codex/tabs/MyWorkbenchTab.tsx` |

## Stats

 2 files changed, 143 insertions(+), 23 deletions(-)
