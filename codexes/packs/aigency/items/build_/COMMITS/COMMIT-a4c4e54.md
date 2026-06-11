# Commit Brief: `a4c4e54` — fix(content-qube): forward personaId in useContentQubeSeries fetch

| Field | Value |
|-------|-------|
| SHA | [`a4c4e54`](https://github.com/Kn0w-1/AigentZBeta/commit/a4c4e541ec2e90532f34bb758dcc363a3b1f344b) |
| Author | Claude |
| Date | 2026-05-14T15:47:46Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix(content-qube): forward personaId in useContentQubeSeries fetch

The codex iframe doesn't reliably carry the session cookie to the registry
API, so getActivePersona(req) was falling back to the caller's first owned
persona (or null) — producing persona_owns=false for every qube even when
the user actually owned them. All scrolls + characters were rendering
locked and routing to the payment gateway.

Matches the existing pattern in useKnytPurchases which explicitly passes
?personaId= in the fetch URL. The receiving API's getActivePersona
already reads ?personaId= from the URL (priority 3 in the resolver chain).

Also includes personaId in the module-level cache key so two personas
hitting the same series get distinct cache entries.
```

## Body

The codex iframe doesn't reliably carry the session cookie to the registry
API, so getActivePersona(req) was falling back to the caller's first owned
persona (or null) — producing persona_owns=false for every qube even when
the user actually owned them. All scrolls + characters were rendering
locked and routing to the payment gateway.

Matches the existing pattern in useKnytPurchases which explicitly passes
?personaId= in the fetch URL. The receiving API's getActivePersona
already reads ?personaId= from the URL (priority 3 in the resolver chain).

Also includes personaId in the module-level cache key so two personas
hitting the same series get distinct cache entries.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/CharactersTab.tsx` |
| Modified | `app/triad/components/codex/tabs/ScrollsTab.tsx` |
| Modified | `app/triad/components/codex/tabs/useContentQubeSeries.ts` |

## Stats

 3 files changed, 20 insertions(+), 4 deletions(-)
