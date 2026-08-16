# Current State

## Current verified state

Static repository evidence confirms:

- One inactive, manual-trigger Google Sheets proof-of-concept workflow exists.
- The POC contains an n8n Google Sheets OAuth credential ID/name reference; it contains no OAuth token or client secret.
- The POC reads the `SLOTS` sheet for one hardcoded date/time and checks whether `Status` equals `Available`.
- On the available branch, the POC is configured to change that slot to `Booked` and append fixed test booking data to `Sheet1`.
- The POC workflow JSON parses successfully and remains preserved as historical evidence.
- `01-physiobot-intake.json` is implemented as a manual-trigger, read-only Google Sheets workflow.
- The new workflow reads `SLOTS`, returns all rows matching `Status = Available`, and does not filter by date or time.
- Static validation confirms that the new workflow contains no Google Sheets write operation.

This checkout contains no execution history, so live Google Sheets connectivity and successful runtime execution have not been independently reverified here. The original checklist records earlier setup and row-creation success on the configured environment.

## Current iteration

Iteration 1 — Dynamic Availability

## Implementation

Complete

## Runtime verification

Pending on the configured n8n instance

## Next planned iteration

Iteration 2 — WhatsApp Connectivity

Do not begin until Iteration 1 runtime acceptance tests pass.

## Not implemented yet

- WhatsApp integration, connectivity, and interactive buttons.
- Dynamic date/time menus.
- n8n Data Table conversation state.
- Pending booking intake.
- Clinic decision and confirmation automation.
- Patient and provider notifications.
- Appointment reminders.
