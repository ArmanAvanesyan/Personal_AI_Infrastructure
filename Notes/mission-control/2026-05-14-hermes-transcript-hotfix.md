# Mission Control Hermes transcript hotfix

**Date:** 2026-05-14
**Scope:** Mission Control / Hermes profile-aware transcript lookup
**Status:** Applied live and pushed to fork

## Summary
Mission Control was showing merged Adolf Hermes sessions in `/api/sessions`, but historical transcript reads through `/api/sessions/transcript?kind=hermes...` could still return empty results.

Root cause: the Hermes transcript route was still hardcoded to read only the legacy root DB path:
- `~/.hermes/state.db`

while the active Hermes runtime had already moved to the profile-aware DB path:
- `~/.hermes/profiles/adolf/state.db`

## Fix
Source fix applied in Mission Control:
- export `getHermesDbPaths()` from `src/lib/hermes-sessions.ts`
- update `src/app/api/sessions/transcript/route.ts` to try Hermes DB candidates in profile-aware order instead of using only the root DB

## Deployment result
Applied live on the running Mission Control instance and verified against historical Adolf sessions.

Verified examples:
- `20260513_212535_4ea51cb0`
- `20260513_203935_e0c4deec`
- `20260513_175618_3ca691ae`

These sessions now return transcript payloads through the app-facing API instead of `[]`.

## Source control
Mission Control fork branch:
- `ArmanAvanesyan/mission-control`
- branch: `hotfix/hermes-profile-aware-runtime`

Mission Control commit:
- `58f44f21d28be815b257495e3f284171991d1cb1`
- `Fix Hermes transcript lookup for active profiles`

## Notes
This is a tracking note in Personal_AI_Infrastructure only.
The actual code change belongs in the Mission Control fork/upstream PR flow.
