# PhysioBot Development Iterations

Work on one iteration at a time. A later iteration begins only when explicitly requested; acceptance criteria live in [TESTING.md](TESTING.md).

## Iteration 0 — Repository Foundation

- Inspect existing work.
- Preserve the Google Sheets proof of concept.
- Scaffold the repository.
- Establish development and security rules.
- Organize MVP documentation.

## Iteration 1 — Dynamic Availability

Goal:

```text
Manual Trigger
    ↓
Read SLOTS
    ↓
Return all Status = Available rows
```

Acceptance criteria:

- Reads the existing `SLOTS` sheet.
- Returns available rows dynamically.
- Does not hardcode date or time.
- Does not modify slots.
- Does not create bookings.
- Does not add WhatsApp behavior.

## Iteration 2 — WhatsApp Connectivity

Goal:

```text
Patient sends WhatsApp message
        ↓
n8n receives it
        ↓
PhysioBot responds
```

Add initial interactive `Book Appointment` button/menu behavior.

## Iteration 3 — Dynamic Date and Time Menus

Read availability from `SLOTS` and display it through WhatsApp interactive menus:

```text
Available Dates
        ↓
Available Times
```

## Iteration 4 — Conversation State

Introduce n8n Data Tables and collect:

- Patient name.
- Appointment type.
- Provider preference.
- Selected date and time.
- Reason for visit.
- Home address when applicable.

## Iteration 5 — Pending Booking

Create a Google Sheets booking row with `Status = Pending` and send:

```text
Appointment request received.
Your appointment is not confirmed yet.
```

Submitting a request must not mark the selected slot booked or unavailable.

## Iteration 6 — Clinic Decision and Notifications

The clinic manually sets the confirmed date, confirmed time, assigned provider, and status.

When `Status = Confirmed`:

- Notify the patient.
- Notify the assigned provider.
- Mark the relevant slot unavailable/booked if appropriate.

When `Status = Rejected`, notify the patient. Appointment changes remain `Confirmed` and trigger an update notification.

## Iteration 7 — Appointment Reminders

- Add an approximately 24-hour reminder.
- Add an approximately 2-hour reminder.
- Prevent duplicate sends.

## Iteration 8 — Automated Availability Generation

**Status:** Planned / Requires clinic workflow validation

Potential future flow:

```text
Recurring working hours
        +
Schedule exceptions
        ↓
Generated future slots
```

This concept requires validation of the clinic's real scheduling workflow before implementation. Do not create supporting sheets or workflows until that validation is complete and Iteration 8 is explicitly requested.
