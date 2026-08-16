# Repository Instructions for Codex Agents

## Required context

Before changing this repository:

1. Read `docs/PROJECT_CONTEXT.md` in full.
2. Read `docs/CURRENT_STATE.md` in full.
3. Read `docs/ITERATIONS.md` and the relevant acceptance criteria in `docs/TESTING.md`.
4. Read every existing workflow relevant to the requested change.
5. Inspect Git status and preserve unrelated user changes.

## Working rules

- Work incrementally and implement only the iteration explicitly requested.
- Preserve previously working behavior and historical POCs.
- Prefer the smallest change that satisfies the current acceptance criteria.
- Never store credentials, access tokens, API keys, app secrets, OAuth secrets, private keys, or other secret values in Git.
- Do not replace working n8n credential references unnecessarily. Credential ID/name references may remain in exported workflows; credential values stay in the n8n instance.
- Never automatically confirm appointments.
- Never automatically assign physiotherapists.
- Never treat a patient-requested slot as confirmed.
- Never mark a requested slot booked or unavailable until explicit clinic-confirmation logic requires it.
- Keep Google Sheets as the operational source of truth for the MVP.
- Use n8n Data Tables only for temporary conversation state.
- Do not introduce a custom backend, PostgreSQL, NestJS, BullMQ, Redis, or other infrastructure unless explicitly approved.
- Do not add AI or LLM behavior unless explicitly requested.
- Validate every modified workflow JSON file before finishing.
- Report assumptions instead of silently inventing business rules.

## Required task report

End every future task with:

```text
What changed
What was not changed
How to test it
Expected result
Files changed
Assumptions
Known limitations
Next iteration — not implemented
```
