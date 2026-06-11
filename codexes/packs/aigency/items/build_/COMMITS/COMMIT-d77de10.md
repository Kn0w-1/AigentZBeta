# Commit Brief: `d77de10` — pack browser: auto-link inline-code doc references to sibling files

| Field | Value |
|-------|-------|
| SHA | [`d77de10`](https://github.com/Kn0w-1/AigentZBeta/commit/d77de10e53befda469cf3ddeda705f824feb50cb) |
| Author | Claude |
| Date | 2026-05-21T21:31:46Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
pack browser: auto-link inline-code doc references to sibling files

In PackBrowserTab (the codex Docs surface used by KNYT Wheel, AgentiQ
updates, etc.), inline-code spans whose text matches a known item
basename in the pack are now rendered as clickable buttons that switch
the browser to that item — including switching the active collection
when the referenced item lives in a different collection than the
current one.

This makes existing markdown like:

    1. `KNYT_CAMPAIGN_OPERATOR_BRIEF.md`
    2. `KNYT_CAMPAIGN_ACTIVATION_BLUEPRINT.md`

linkable without rewriting any source doc. Lookup is per-pack
(basename → { path, collectionId }) computed once after collections
hydrate, so the surface stays canonical-content-driven — adding a new
file to collections.json is enough to make existing refs to it light
up.
```

## Body

In PackBrowserTab (the codex Docs surface used by KNYT Wheel, AgentiQ
updates, etc.), inline-code spans whose text matches a known item
basename in the pack are now rendered as clickable buttons that switch
the browser to that item — including switching the active collection
when the referenced item lives in a different collection than the
current one.

This makes existing markdown like:

    1. `KNYT_CAMPAIGN_OPERATOR_BRIEF.md`
    2. `KNYT_CAMPAIGN_ACTIVATION_BLUEPRINT.md`

linkable without rewriting any source doc. Lookup is per-pack
(basename → { path, collectionId }) computed once after collections
hydrate, so the surface stays canonical-content-driven — adding a new
file to collections.json is enough to make existing refs to it light
up.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/PackBrowserTab.tsx` |

## Stats

 1 file changed, 65 insertions(+), 1 deletion(-)
