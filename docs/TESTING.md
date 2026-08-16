# PhysioBot Testing

This file separates implementation work from pass/fail acceptance criteria. A checked item must be supported by repository evidence or a recorded execution on the configured n8n machine.

## Historical setup record

The original build checklist recorded the following as completed on the separate working environment. These are historical claims, not runtime verification from this checkout:

- n8n installed, launched, and able to create and execute workflows.
- MVP Google Sheet and Google Cloud project created.
- Google Sheets API enabled and credentials connected to n8n.
- A row was created from n8n.

The original checklist left WhatsApp/Meta setup and message sending unverified. Credentials, tokens, and live account access are intentionally outside this repository.

## Iteration 0 — Repository Foundation

### Implementation tasks

- [x] Inventory all existing project files and Git history.
- [x] Preserve the Google Sheets POC as a historical workflow.
- [x] Move the main specification to `docs/PROJECT_CONTEXT.md`.
- [x] Integrate the historical checklist into iteration-based testing.
- [x] Add repository and n8n contributor guidance.
- [x] Document current and planned iterations.

### Acceptance criteria

- [x] Repository has the documented Iteration 0 scaffold.
- [x] The POC workflow business logic is unchanged.
- [x] Every committed workflow JSON file parses successfully.
- [x] No empty future workflow placeholders exist.
- [x] No obvious committed secret value is detected by the Iteration 0 scan.
- [x] Iteration 1 functionality was not implemented during Iteration 0.

## Iteration 1 — Dynamic Availability

### Implementation tasks

- [x] Create a new manual-trigger workflow for dynamic availability.
- [x] Read rows from the existing `SLOTS` sheet.
- [x] Filter rows using `Status = Available`.
- [x] Keep the workflow read-only.

### Static validation

- [x] Workflow JSON parses successfully.
- [x] Manual Trigger connects directly to Get Available Slots.
- [x] The Google Sheets operation is explicitly `read`.
- [x] The only filter is `Status = Available`.
- [x] The read operation is configured to return all matching rows.
- [x] No hardcoded appointment date or time filter exists.
- [x] No Google Sheets write operation exists.
- [x] The historical POC remains byte-for-byte unchanged.
- [x] No WhatsApp, conversation-state, or booking nodes exist.

### Runtime acceptance criteria

- [x] Workflow imports successfully into n8n.
- [x] Existing Google Sheets credential can be selected/reused.
- [x] Manual execution succeeds.
- [x] Workflow reads the existing `SLOTS` sheet.
- [x] All `Status = Available` rows are returned.
- [x] Rows with another status are excluded.
- [x] No date is hardcoded.
- [x] No time is hardcoded.
- [x] No `SLOTS` row is modified.
- [x] No booking row is created.
- [x] Adding a new `Available` row to `SLOTS` causes it to appear on the next execution.
- [x] Changing an `Available` slot to unavailable/booked causes it to disappear from the next execution.

## Pre-Iteration 2 — WhatsApp Connectivity Smoke Test

### Static validation

- [x] Workflow JSON parses successfully.
- [x] Outbound and inbound branches are independent.
- [x] The outbound branch contains only a Manual Trigger and WhatsApp send node.
- [x] The inbound branch contains only a WhatsApp Trigger and pass-through Edit Fields node.
- [x] The trigger listens only for message events and retains the raw incoming fields.
- [x] Credential selections and sender ID are intentionally unresolved.
- [x] The recipient uses a non-phone placeholder.
- [x] No Google Sheets, AI, booking, state, or automatic-reply node exists.
- [x] The Iteration 0 and Iteration 1 workflows remain unchanged.

### Runtime acceptance criteria

- [ ] Smoke-test workflow imports successfully.
- [ ] Existing `WhatsApp account` credential can be selected.
- [ ] Meta test sender appears/selects successfully.
- [ ] Outbound test message reaches personal WhatsApp.
- [ ] Existing `WhatsApp OAuth account` credential can be selected.
- [ ] WhatsApp Trigger enters listening/test mode successfully.
- [ ] Sending `hello` to the Meta test number creates an n8n execution.
- [ ] Incoming payload contains the real WhatsApp message event.
- [ ] No Google Sheet data is touched.
- [ ] No booking data is created.

## Iteration 2 — WhatsApp Connectivity

### Implementation tasks

- [ ] Configure the WhatsApp webhook/trigger in the working n8n instance.
- [ ] Receive an incoming patient message.
- [ ] Send a response with initial `Book Appointment` interactive behavior.

### Acceptance criteria

- [ ] A test WhatsApp message reaches n8n.
- [ ] PhysioBot responds to the same test patient.
- [ ] The initial booking action is presented interactively.
- [ ] No appointment is automatically confirmed or assigned.

## Iteration 3 — Dynamic Date and Time Menus

### Implementation tasks

- [ ] Read requestable dates and times from `SLOTS`.
- [ ] Build interactive date and time menus.

### Acceptance criteria

- [ ] Only dates with available slots are displayed.
- [ ] Selecting a date displays only available times for that date.
- [ ] Changing `SLOTS` changes the menus without editing workflow data.
- [ ] Selecting a slot does not modify it or confirm an appointment.

## Iteration 4 — Conversation State

### Implementation tasks

- [ ] Add temporary state in n8n Data Tables.
- [ ] Collect name, appointment type, provider preference, date, time, and reason.
- [ ] Collect an address only for a home visit.
- [ ] Add booking review and restart behavior.

### Acceptance criteria

- [ ] A patient can progress through every required prompt.
- [ ] State resumes at the correct step for the same patient.
- [ ] Clinic visits skip the address prompt.
- [ ] Provider preference does not assign a provider.
- [ ] Invalid or expired state can restart safely.

## Iteration 5 — Pending Booking

### Implementation tasks

- [ ] Validate required booking fields.
- [ ] Generate a unique appointment ID and timestamps.
- [ ] Append a booking row to Google Sheets with `Status = Pending`.
- [ ] Clear temporary state after a successful write.
- [ ] Make Google Sheets failures visible in n8n.

### Acceptance criteria

- [ ] One submitted request creates one booking row.
- [ ] The booking status is `Pending`.
- [ ] The acknowledgement says the appointment is not confirmed yet.
- [ ] The requested slot is not marked booked or unavailable.
- [ ] Duplicate webhook processing does not intentionally create duplicate bookings.
- [ ] A failed sheet write is visible and does not report success to the patient.

## Iteration 6 — Clinic Decision and Notifications

### Implementation tasks

- [ ] Detect relevant booking updates.
- [ ] Support clinic-entered confirmed date/time, assigned provider, notes, and status.
- [ ] Send confirmation, rejection, appointment-update, and provider-assignment notices.
- [ ] Mark the relevant slot unavailable/booked when clinic confirmation requires it.
- [ ] Record notification/update timestamps or flags needed for duplicate protection.

### Acceptance criteria

- [ ] Only a clinic-set `Confirmed` status triggers confirmation.
- [ ] Provider assignment comes only from the clinic-maintained sheet.
- [ ] `Rejected` notifies the patient without confirming the request.
- [ ] A changed confirmed appointment remains `Confirmed` and sends an update.
- [ ] The relevant slot changes only during explicit clinic confirmation logic.
- [ ] Notification failures are visible in n8n.

## Iteration 7 — Appointment Reminders

### Implementation tasks

- [ ] Identify upcoming confirmed appointments.
- [ ] Add approximately 24-hour and 2-hour reminder paths.
- [ ] Add duplicate-send flags and failure visibility.

### Acceptance criteria

- [ ] Only `Confirmed` appointments receive reminders.
- [ ] The 24-hour reminder sends once in its configured window.
- [ ] The 2-hour reminder sends once in its configured window.
- [ ] Sent flags prevent duplicate reminders.
- [ ] Failed sends remain visible for recovery or retry.

## End-to-end MVP acceptance

- [ ] Patient completes intake through WhatsApp.
- [ ] Available date/time choices come from `SLOTS`.
- [ ] Google Sheets receives a `Pending` request.
- [ ] Clinic review, assignment, confirmation, rejection, and updates work.
- [ ] Patient and provider notifications are delivered as applicable.
- [ ] Reminder automation passes both timing tests without duplicates.
