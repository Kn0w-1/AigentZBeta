# Commit Brief: `d4e1dfd` — fix(ask-agent): self-referential TDZ bug + add MoneyPenny/Metayé to valid specialists

| Field | Value |
|-------|-------|
| SHA | [`d4e1dfd`](https://github.com/Kn0w-1/AigentZBeta/commit/d4e1dfd5b544c66c2fd66aadc0f78438a01cad0e) |
| Author | Claude |
| Date | 2026-05-13T18:33:14Z |
| Branch | dev (direct push) |
| Type | `fix` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
fix(ask-agent): self-referential TDZ bug + add MoneyPenny/Metayé to valid specialists

Line 86 read 'const resolvedSpecialistId = resolveSpecialistId(resolvedSpecialistId)'
— the function call argument referenced the variable being declared, so
every request threw ReferenceError (Cannot access 'resolvedSpecialistId'
before initialization). That blew up inside the try block and bubbled
out as a generic 500. Fix: pass body.specialistId as intended.

Also extend VALID_SPECIALISTS with 'moneypenny' and 'metaye' so the
two new agents pass validation, plus aliases for 'money-penny',
'aigent-moneypenny', 'metayé', 'aigent-metaye'.
```

## Body

Line 86 read 'const resolvedSpecialistId = resolveSpecialistId(resolvedSpecialistId)'
— the function call argument referenced the variable being declared, so
every request threw ReferenceError (Cannot access 'resolvedSpecialistId'
before initialization). That blew up inside the try block and bubbled
out as a generic 500. Fix: pass body.specialistId as intended.

Also extend VALID_SPECIALISTS with 'moneypenny' and 'metaye' so the
two new agents pass validation, plus aliases for 'money-penny',
'aigent-moneypenny', 'metayé', 'aigent-metaye'.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `app/api/assistant/ask-agent/route.ts` |

## Stats

 2 files changed, 8 insertions(+), 3 deletions(-)
