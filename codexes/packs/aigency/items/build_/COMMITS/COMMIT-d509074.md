# Commit Brief: `d509074` — myCanvas: visible hydration diagnostics in experience panels

| Field | Value |
|-------|-------|
| SHA | [`d509074`](https://github.com/Kn0w-1/AigentZBeta/commit/d5090747869c93d64d0eef321f2261dc0ebaa73f) |
| Author | Claude |
| Date | 2026-05-22T08:13:46Z |
| Branch | dev (direct push) |
| Type | `push` |
| Repo | Kn0w-1/AigentZBeta |

## Commit Message

```
myCanvas: visible hydration diagnostics in experience panels

Body content isn't appearing in the right panel after selecting an
entry. From outside I can't tell whether:
  (a) the hydration effect isn't firing,
  (b) the GET /[id] is returning a non-200 (likely 404 from persona
      mismatch between list and detail resolution), or
  (c) the entry is genuinely empty (old origin rows created before
      the 'save full origin' change had bodyMd='' + metaJson={
      experienceId } only).

This commit ONLY adds diagnostics — no behaviour changes. Each
experience panel renders a HydrationIndicator under the title that
shows:
  • 'no hydration attempted'   → effect never fired (case a)
  • 'Loading full entry…'      → fetch in flight
  • 'Hydration failed (HTTP X)' → fetch failed (case b — shows code)
  • nothing                    → fetch succeeded; if body is still
                                  empty it's case c

Console.error is also emitted on failure with { entryId, status }
so the browser DevTools log carries the same signal.

Next click after this deploys will tell us which of (a)/(b)/(c) is
the actual cause, and the fix can be surgical from there.
```

## Body

Body content isn't appearing in the right panel after selecting an
entry. From outside I can't tell whether:
  (a) the hydration effect isn't firing,
  (b) the GET /[id] is returning a non-200 (likely 404 from persona
      mismatch between list and detail resolution), or
  (c) the entry is genuinely empty (old origin rows created before
      the 'save full origin' change had bodyMd='' + metaJson={
      experienceId } only).

This commit ONLY adds diagnostics — no behaviour changes. Each
experience panel renders a HydrationIndicator under the title that
shows:
  • 'no hydration attempted'   → effect never fired (case a)
  • 'Loading full entry…'      → fetch in flight
  • 'Hydration failed (HTTP X)' → fetch failed (case b — shows code)
  • nothing                    → fetch succeeded; if body is still
                                  empty it's case c

Console.error is also emitted on failure with { entryId, status }
so the browser DevTools log carries the same signal.

Next click after this deploys will tell us which of (a)/(b)/(c) is
the actual cause, and the fix can be surgical from there.

## Files Changed

| Change | File |
|--------|------|
| Modified | `app/triad/components/codex/tabs/MyCanvasTab.tsx` |

## Stats

 1 file changed, 57 insertions(+), 7 deletions(-)
