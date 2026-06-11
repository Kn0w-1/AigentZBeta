# Commit Brief: `44ff2b0` — aigentMe welcome: fix queued template, move request-access into chip strip, sequence chip dispatch on send

| Field | Value |
|-------|-------|
| SHA | [`44ff2b0`](https://github.com/Kn0w-1/AigentZBeta/commit/44ff2b071f7846a8f20c06fc4a6c2ca0030274fc) |
| Author | Claude |
| Date | 2026-05-26T18:17:49Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
aigentMe welcome: fix queued template, move request-access into chip strip, sequence chip dispatch on send

Three fixes from the feedback pass.

#1 — Queued action template now also renders when an NBE comes from
the Brief surface, not just moveForward. The lookup in
WelcomeRightPane was only checking moveForwardResult so queued
intents from brief.nextBestActions silently rendered nothing. Now
falls back to brief.nextBestActions when the moveForward lookup
returns null.

#2 — Request access button moved out of the absolute-positioned
overlay (which overlapped the right pane's close affordance) and
into the copilot chip strip as a highlighted pulse chip. The chip
appears only when the active persona has no admin grants — same gate
as before, more discoverable surface. RequestAdminAccessButton now
supports a controlled mode (open + onOpenChange props) so the chip
can drive it.

#4 — Quick-chip sequencing rewrite. Previously: chip click fired the
right-pane fetch AND auto-sent the chat 100ms later. Result: the
chat raced ahead of the brief data, the LLM got no ground truth, and
the response invented '[Priority 1]' placeholders even with the
groundContext plumbing in place.

New model:
  - Chip click sets the input + fires onSelect (sync layout switch
    to skeleton state only). Does NOT auto-send.
  - Chip carries onDispatchOnSend (async fetch dispatcher) that
    runs INSIDE handleSend, BEFORE the chat POST. Captured into
    pendingDispatchRef on chip click.
  - User is free to edit the prompt before pressing Send.
  - Pressing Send awaits the dispatch (right-pane fetch lands +
    groundContext updates) THEN fires the chat with fresh data.

Net effect: both panes stay in sync, the LLM sees the actual brief
shape, and the operator can amend the prompt before firing.

Chip strip render adds a highlight tone (emerald ring + slow pulse)
for chips with highlight: true, and grew from 4 to 5 visible slots.
```

## Body

Three fixes from the feedback pass.

#1 — Queued action template now also renders when an NBE comes from
the Brief surface, not just moveForward. The lookup in
WelcomeRightPane was only checking moveForwardResult so queued
intents from brief.nextBestActions silently rendered nothing. Now
falls back to brief.nextBestActions when the moveForward lookup
returns null.

#2 — Request access button moved out of the absolute-positioned
overlay (which overlapped the right pane's close affordance) and
into the copilot chip strip as a highlighted pulse chip. The chip
appears only when the active persona has no admin grants — same gate
as before, more discoverable surface. RequestAdminAccessButton now
supports a controlled mode (open + onOpenChange props) so the chip
can drive it.

#4 — Quick-chip sequencing rewrite. Previously: chip click fired the
right-pane fetch AND auto-sent the chat 100ms later. Result: the
chat raced ahead of the brief data, the LLM got no ground truth, and
the response invented '[Priority 1]' placeholders even with the
groundContext plumbing in place.

New model:
  - Chip click sets the input + fires onSelect (sync layout switch
    to skeleton state only). Does NOT auto-send.
  - Chip carries onDispatchOnSend (async fetch dispatcher) that
    runs INSIDE handleSend, BEFORE the chat POST. Captured into
    pendingDispatchRef on chip click.
  - User is free to edit the prompt before pressing Send.
  - Pressing Send awaits the dispatch (right-pane fetch lands +
    groundContext updates) THEN fires the chat with fresh data.

Net effect: both panes stay in sync, the LLM sees the actual brief
shape, and the operator can amend the prompt before firing.

Chip strip render adds a highlight tone (emerald ring + slow pulse)
for chips with highlight: true, and grew from 4 to 5 visible slots.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/AigentMeWelcomeSplitTab.tsx` |
| Modified | `components/metame/admin/RequestAdminAccessButton.tsx` |
| Modified | `components/metame/welcome/WelcomeRightPane.tsx` |
| Modified | `components/smarttriad/copilot/SmartTriadCopilotLayer.tsx` |

## Stats

 4 files changed, 174 insertions(+), 63 deletions(-)
