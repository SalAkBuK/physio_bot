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
- The user reports that Iteration 1 passed its runtime acceptance tests on the configured n8n instance.
- `99-whatsapp-credential-smoke-test.json` is implemented with independent outbound and inbound WhatsApp test branches.

This checkout contains no WhatsApp execution history. WhatsApp connectivity remains unverified until the smoke test runs on the configured n8n instance.

## Iteration 1

PASSED

## Pre-Iteration 2 WhatsApp Connectivity Smoke Test

Workflow implemented

Runtime verification pending

## Iteration 2

NOT STARTED

## Not implemented yet

- Iteration 2 WhatsApp response flow and interactive buttons.
- Dynamic date/time menus.
- n8n Data Table conversation state.
- Pending booking intake.
- Clinic decision and confirmation automation.
- Patient and provider notifications.
- Appointment reminders.
