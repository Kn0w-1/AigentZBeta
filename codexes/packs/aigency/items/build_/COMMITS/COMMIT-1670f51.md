# Commit Brief: `1670f51` — doc: ContentQube registry as SOT — shelf + tab canonicalization writeup

| Field | Value |
|-------|-------|
| SHA | [`1670f51`](https://github.com/Kn0w-1/AigentZBeta/commit/1670f51a3abe24d69bcf8d925582e0f46e88ca89) |
| Author | Claude |
| Date | 2026-05-14T18:20:50Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
doc: ContentQube registry as SOT — shelf + tab canonicalization writeup

Documents the four parallel ownership paths and the Phase A + Phase B
remediation that consolidates them onto the ContentQube registry. Covers:

- Root cause analysis (RC-1 through RC-6 from the plan)
- Phase A surgical fix (variant-aware OwnedIssue + KnytTab predicates)
- Phase B canonicalization (new /series-rights endpoint, new hook,
  KnytShelfTab + ScrollsTab + CharactersTab migration, KnytTab overlay)
- Operator verification checklist for arkagent@knyt on dev-beta
- Phase C backlog (per-rarity ownership, legacy path removal,
  Qriptopian pilot, chain-mint activation)

Registered under col_updates in codexes/packs/agentiq/collections.json.
Includes .amplify-deploy trigger.
```

## Body

Documents the four parallel ownership paths and the Phase A + Phase B
remediation that consolidates them onto the ContentQube registry. Covers:

- Root cause analysis (RC-1 through RC-6 from the plan)
- Phase A surgical fix (variant-aware OwnedIssue + KnytTab predicates)
- Phase B canonicalization (new /series-rights endpoint, new hook,
  KnytShelfTab + ScrollsTab + CharactersTab migration, KnytTab overlay)
- Operator verification checklist for arkagent@knyt on dev-beta
- Phase C backlog (per-rarity ownership, legacy path removal,
  Qriptopian pilot, chain-mint activation)

Registered under col_updates in codexes/packs/agentiq/collections.json.
Includes .amplify-deploy trigger.

## Files Changed

| Change | File |
|--------|------|
| Modified | `.amplify-deploy` |
| Modified | `codexes/packs/agentiq/collections.json` |
| Added | `codexes/packs/agentiq/updates/2026-05-14_contentqube-registry-as-sot-shelf-tab-canonicalization.md` |

## Stats

 3 files changed, 167 insertions(+), 2 deletions(-)
