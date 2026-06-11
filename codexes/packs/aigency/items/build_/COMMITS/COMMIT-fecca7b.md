# Commit Brief: `fecca7b` — shell: render wallet/drawer actions as overlays, suppress companion LLM prompt

| Field | Value |
|-------|-------|
| SHA | [`fecca7b`](https://github.com/Kn0w-1/AigentZBeta/commit/fecca7bba9f7e110dd5210a6e4f01f27217e9376) |
| Author | Claude |
| Date | 2026-05-22T05:34:31Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
shell: render wallet/drawer actions as overlays, suppress companion LLM prompt

Clicking wallet/settings/connections/memory/identity/persona in the
platform Next.js runtime shell was dispatching two side-effects:
  (1) postShellEvent('MENU_ACTION', { action_id, prompt, ... })
  (2) aaClient.postMenuAction(...) — server roundtrip that can echo
      back a menu_event.prompt the shell then forwards to the runtime

The companion prompt caused the runtime to call handlePrompt and
reset state when the user just wanted to peek at the wallet (e.g.
to check the active persona or sign in).

Add DRAWER_ONLY_MENU_ACTIONS set and short-circuit handleMenuAction
for those ids: post a clean MENU_ACTION with only { action_id, payload }
and skip the AA roundtrip entirely. The runtime's existing
DRAWER_ACTION_HANDLERS early-return path is now the only handler that
fires, so the drawer overlays without disturbing runtime state.

Same fix needs to ship in Lovable's thin client — instruction sent
separately.
```

## Body

Clicking wallet/settings/connections/memory/identity/persona in the
platform Next.js runtime shell was dispatching two side-effects:
  (1) postShellEvent('MENU_ACTION', { action_id, prompt, ... })
  (2) aaClient.postMenuAction(...) — server roundtrip that can echo
      back a menu_event.prompt the shell then forwards to the runtime

The companion prompt caused the runtime to call handlePrompt and
reset state when the user just wanted to peek at the wallet (e.g.
to check the active persona or sign in).

Add DRAWER_ONLY_MENU_ACTIONS set and short-circuit handleMenuAction
for those ids: post a clean MENU_ACTION with only { action_id, payload }
and skip the AA roundtrip entirely. The runtime's existing
DRAWER_ACTION_HANDLERS early-return path is now the only handler that
fires, so the drawer overlays without disturbing runtime state.

Same fix needs to ship in Lovable's thin client — instruction sent
separately.

## Files Changed

| Change | File |
|--------|------|
| Modified | `apps/metame-runtime-shell/app/page.tsx` |

## Stats

 1 file changed, 24 insertions(+)
