# Commit Brief: `7e82f93` — fix empty-body 500 on generate: guard getActivePersona + defensive json parse

| Field | Value |
|-------|-------|
| SHA | [`7e82f93`](https://github.com/Kn0w-1/AigentZBeta/commit/7e82f93585a6ea945b3b68364aa9fd0438c9d18c) |
| Author | Claude |
| Date | 2026-05-22T04:58:19Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix empty-body 500 on generate: guard getActivePersona + defensive json parse

getActivePersona() was unguarded — if the spine threw, Next.js returned
an empty 500 body and res.json() raised 'unexpected end of data', which
surfaced raw in the UI. Fix: wrap spine call in try/catch so failures
fall through to the existing 401 guard.

RemixDialog.submit() now catches json() parse failures separately and
shows a readable 'Generation failed — please try again' message instead
of the raw JSON parse exception string.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym
```

## Body

getActivePersona() was unguarded — if the spine threw, Next.js returned
an empty 500 body and res.json() raised 'unexpected end of data', which
surfaced raw in the UI. Fix: wrap spine call in try/catch so failures
fall through to the existing 401 guard.

RemixDialog.submit() now catches json() parse failures separately and
shows a readable 'Generation failed — please try again' message instead
of the raw JSON parse exception string.

https://claude.ai/code/session_01WpEKdSdKfopL9QdLAKiEym

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/api/community-content/generate/route.ts` |
| Modified | `components/metame/runtime/RemixDialog.tsx` |

## Stats

 2 files changed, 14 insertions(+), 4 deletions(-)
