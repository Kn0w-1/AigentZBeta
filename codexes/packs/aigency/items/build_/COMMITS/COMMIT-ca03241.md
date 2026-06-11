# Commit Brief: `ca03241` — community: image proxy endpoint so card thumbs render without bloating list payload

| Field | Value |
|-------|-------|
| SHA | [`ca03241`](https://github.com/Kn0w-1/AigentZBeta/commit/ca03241ca65c64d05c505faad58f27c4ae5f87f7) |
| Author | Claude |
| Date | 2026-05-23T08:06:26Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
community: image proxy endpoint so card thumbs render without bloating list payload

After stripping image_url from /api/community-content/list to avoid
413s on Lambda's 6 MB payload ceiling, card thumbnails disappeared
(no source). Restore them via a per-id proxy:

  GET /api/community-content/[id]/image
    • Reads community_generated_content.image_url
    • Refuses non-public statuses (draft / pending_promotion / rejected)
    • Decodes the data:image/...;base64,... payload to raw bytes
    • Serves with Content-Type from the data URL prefix + 24h cache
    • Handles already-HTTPS URLs via 302 redirect (forward-compat for
      when generated images move to Supabase Storage)

Cards (KnytCommunityContentTab.ContentCard) and the detail view both
now <img src="/api/community-content/<id>/image"> with an onError
fallback to the existing skill-icon placeholder. The 24h immutable
cache header means once a card thumb loads, re-opening its detail
view pulls the same image from browser cache instantly — no double
network hit.

Long-term backlog item: upload generated images to Supabase Storage
on creation and persist the https URL instead of base64. The proxy
already handles that case (302 redirect path) so the cutover will be
zero-touch on the client. Captured in the 2026-05-22 backlog brief.
```

## Body

After stripping image_url from /api/community-content/list to avoid
413s on Lambda's 6 MB payload ceiling, card thumbnails disappeared
(no source). Restore them via a per-id proxy:

  GET /api/community-content/[id]/image
    • Reads community_generated_content.image_url
    • Refuses non-public statuses (draft / pending_promotion / rejected)
    • Decodes the data:image/...;base64,... payload to raw bytes
    • Serves with Content-Type from the data URL prefix + 24h cache
    • Handles already-HTTPS URLs via 302 redirect (forward-compat for
      when generated images move to Supabase Storage)

Cards (KnytCommunityContentTab.ContentCard) and the detail view both
now <img src="/api/community-content/<id>/image"> with an onError
fallback to the existing skill-icon placeholder. The 24h immutable
cache header means once a card thumb loads, re-opening its detail
view pulls the same image from browser cache instantly — no double
network hit.

Long-term backlog item: upload generated images to Supabase Storage
on creation and persist the https URL instead of base64. The proxy
already handles that case (302 redirect path) so the cutover will be
zero-touch on the client. Captured in the 2026-05-22 backlog brief.

## Files Changed

| Change | File |
|--------|------|
| Added | `app/api/community-content/[id]/image/route.ts` |
| Modified | `app/triad/components/codex/tabs/KnytCommunityContentTab.tsx` |

## Stats

 2 files changed, 103 insertions(+), 4 deletions(-)
