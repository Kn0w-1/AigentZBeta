# Commit Brief: `befbd12` — persona-resolution propagation + multi-persona publish ownership + community list 413

| Field | Value |
|-------|-------|
| SHA | [`befbd12`](https://github.com/Kn0w-1/AigentZBeta/commit/befbd125f2e2207dbbd9e8acd5667506736b1e63) |
| Author | Claude |
| Date | 2026-05-23T07:35:41Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
persona-resolution propagation + multi-persona publish ownership + community list 413

Three interrelated fixes to the persona-resolution work:

1) useActivePersona attaches x-persona-id from localStorage.
   The hook calls /api/wallet/active-persona via raw fetch (not
   personaFetch), so the resolver was falling to step 4 and
   returning devagent regardless of the user's wallet-side switch.
   Mirror personaFetch's x-persona-id behaviour here so cartridge
   greetings, balances, and all downstream surfaces consuming the
   active-persona surface see the correct persona. Same fix pattern
   that already shipped in personaFetch — getActivePersona priority
   2 honours x-persona-id when the persona is owned.

2) publish + discard accept auth-profile-level ownership instead
   of strict personaId equality.
   When marketa creates a remix and dele's session resolves to
   devagent at publish time (stale prop chain or persona switch
   mid-session), strict 'creator_persona_id !== personaId' rejects
   the legitimate author with 'Not your content'. New
   callerOwnsCreator() helper resolves the caller's auth profile
   + merged links, looks up the creator_persona_id under those,
   and accepts the publish/discard iff the caller's auth profile
   owns it. Multi-persona users can manage any of their own remixes
   regardless of which persona is active at the time.

3) Community list strip body+image to avoid 413.
   /api/community-content/list was SELECTing article_body
   (600-900 words) AND image_url (base64 data URLs, 100KB-1MB
   each). Cumulative response exceeded Lambda's 6 MB ceiling and
   the route returned empty body, surfacing as 'JSON.parse:
   unexpected end of data' on the Order of Metayé community tab.
   Same fix pattern that already shipped for the myCanvas list:
   strip the heavy fields, lazy-load on item open. Also wrapped
   the client's res.json() in try/catch so future empty-body
   scenarios render a readable error.

Net result for dele@metame.com:
  • aigentMe greeting / cartridge header reflect the wallet-side
    persona switch (not devagent default)
  • Publishing remixes works regardless of which persona resolved
    at publish time, as long as the caller's auth profile owns
    the creator persona
  • Community tab loads even with many shared remixes
```

## Body

Three interrelated fixes to the persona-resolution work:

1) useActivePersona attaches x-persona-id from localStorage.
   The hook calls /api/wallet/active-persona via raw fetch (not
   personaFetch), so the resolver was falling to step 4 and
   returning devagent regardless of the user's wallet-side switch.
   Mirror personaFetch's x-persona-id behaviour here so cartridge
   greetings, balances, and all downstream surfaces consuming the
   active-persona surface see the correct persona. Same fix pattern
   that already shipped in personaFetch — getActivePersona priority
   2 honours x-persona-id when the persona is owned.

2) publish + discard accept auth-profile-level ownership instead
   of strict personaId equality.
   When marketa creates a remix and dele's session resolves to
   devagent at publish time (stale prop chain or persona switch
   mid-session), strict 'creator_persona_id !== personaId' rejects
   the legitimate author with 'Not your content'. New
   callerOwnsCreator() helper resolves the caller's auth profile
   + merged links, looks up the creator_persona_id under those,
   and accepts the publish/discard iff the caller's auth profile
   owns it. Multi-persona users can manage any of their own remixes
   regardless of which persona is active at the time.

3) Community list strip body+image to avoid 413.
   /api/community-content/list was SELECTing article_body
   (600-900 words) AND image_url (base64 data URLs, 100KB-1MB
   each). Cumulative response exceeded Lambda's 6 MB ceiling and
   the route returned empty body, surfacing as 'JSON.parse:
   unexpected end of data' on the Order of Metayé community tab.
   Same fix pattern that already shipped for the myCanvas list:
   strip the heavy fields, lazy-load on item open. Also wrapped
   the client's res.json() in try/catch so future empty-body
   scenarios render a readable error.

Net result for dele@metame.com:
  • aigentMe greeting / cartridge header reflect the wallet-side
    persona switch (not devagent default)
  • Publishing remixes works regardless of which persona resolved
    at publish time, as long as the caller's auth profile owns
    the creator persona
  • Community tab loads even with many shared remixes

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/[id]/discard/route.ts` |
| Modified | `app/api/community-content/[id]/publish/route.ts` |
| Modified | `app/api/community-content/list/route.ts` |
| Modified | `app/hooks/useActivePersona.ts` |
| Modified | `app/triad/components/codex/tabs/KnytCommunityContentTab.tsx` |

## Stats

 5 files changed, 118 insertions(+), 7 deletions(-)
