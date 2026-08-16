# PhysioBot MVP — Locked Demo Scope v1.1

**Last Updated:** August 16, 2026  
**Status:** Locked for Prototype Build

## 1. Goal

Build a WhatsApp-based appointment automation prototype for Dr. Quratulain's physiotherapy clinic.

The prototype should demonstrate how repetitive appointment administration can be automated while keeping final scheduling and provider assignment under clinic control.

PhysioBot acts as an:

**Automated WhatsApp receptionist**

It does not automatically run the clinic schedule.

The MVP will demonstrate:

- WhatsApp appointment intake
- Interactive buttons and menus
- Showing patients available appointment slots
- Automatic booking-request creation
- Google Sheets booking management
- Manual provider assignment
- Automatic patient confirmations
- Automatic physiotherapist notifications
- Automated appointment reminders

---

# 2. Core Principle

Dr. Quratulain remains the final decision maker.

PhysioBot must never automatically:

- Confirm an appointment
- Assign a physiotherapist
- Override clinic availability
- Diagnose patients
- Provide medical advice

Patients may choose from **available appointment slots**, but choosing a slot creates a **booking request**, not a confirmed appointment.

---

# 3. MVP Architecture

```text
WhatsApp Cloud API
        ↓
       n8n
        ↓
 ┌─────────────────┐
 │                 │
n8n Data Tables   Google Sheets
Temporary         ├── Bookings
Chat State        └── SLOTS / Availability
 │                 │
 └────────┬────────┘
          ↓
WhatsApp Notifications
```

## WhatsApp Cloud API

Used for:

- Receiving messages
- Reply buttons
- Interactive lists
- Patient confirmations
- Provider notifications
- Appointment reminders

## n8n

Used for:

- Conversation logic
- Reading available slots
- Input validation
- Temporary patient state
- Creating booking requests
- Detecting clinic decisions
- Sending notifications
- Running reminder workflows

## n8n Data Tables

Used only for temporary conversation state.

Example:

```text
WhatsApp Number
Current Step
Patient Name
Appointment Type
Provider Preference
Selected Date
Selected Time
Reason
Address
Last Updated
```

## Google Sheets

Google Sheets remains the operational source of truth during the MVP.

The spreadsheet contains two main tabs:

```text
Bookings
SLOTS
```

No custom administrative dashboard will be built.

---

# 4. Primary Users

## Patient

Can:

- Start an appointment request through WhatsApp
- Use buttons and menus instead of typing most answers
- View available appointment dates
- View available appointment times
- Request an available slot
- Receive confirmation once clinic approves
- Receive appointment updates
- Receive reminders
- Contact clinic staff when necessary

Cannot:

- Confirm their own appointment
- Assign their own physiotherapist
- Automatically change confirmed appointments

---

## Dr. Quratulain

Can:

- Control available appointment slots
- Review incoming requests
- Assign physiotherapists
- Change requested date/time if necessary
- Add internal notes
- Confirm requests
- Reject requests
- Change confirmed appointments

Dr. Quratulain remains the final scheduling authority.

---

## Physiotherapist

Can:

- Receive assigned appointments
- Receive appointment details
- Receive relevant assignment notes
- Receive changes to assigned appointments

Cannot:

- Modify the clinic schedule through PhysioBot
- Reassign appointments

---

# 5. WhatsApp UX Principle

PhysioBot should feel similar to a professional WhatsApp Business chatbot.

Use:

- Reply buttons
- Interactive menus
- Short conversational messages
- Text input only when necessary

Avoid:

```text
Type 1 for booking
Type 2 for clinic information
Type 3 for...
```

Buttons and menus are preferred.

---

# 6. Welcome Menu

When a patient starts the conversation:

```text
👋 Welcome to [Clinic Name].

How can we help you today?
```

Buttons:

```text
Book Appointment
Contact Clinic
```

---

# 7. Appointment Intake Flow

## Step 1 — Start Booking

Patient taps:

```text
Book Appointment
```

Bot:

```text
I'd be happy to help you request an appointment.

I'll just need a few details.
```

---

# 8. Patient Name

Bot:

```text
What is your full name?
```

Patient types:

```text
Sarah Ahmed
```

The patient's WhatsApp number is automatically used as the contact number.

The bot does not ask for a phone number unless the clinic later requires an alternative contact number.

---

# 9. Appointment Type

Bot:

```text
Where would you like your appointment?
```

Buttons:

```text
Clinic Visit
Home Visit
```

---

# 10. Provider Preference

Bot:

```text
Do you have a physiotherapist preference?
```

Buttons:

```text
Female Physio
Male Physio
No Preference
```

This is a patient preference only.

It does not assign the provider.

---

# 11. Available Date Selection

Instead of asking the patient to manually enter a preferred date, n8n reads available future appointments from the Google Sheets `SLOTS` tab.

Bot:

```text
📅 Please choose an available date.
```

Example menu:

```text
Choose Date

Monday, 17 August
Tuesday, 18 August
Wednesday, 19 August
Thursday, 20 August
Friday, 21 August
```

Only dates containing requestable appointment slots should appear.

---

# 12. Available Time Selection

After the patient chooses a date, n8n retrieves the available times for that date.

Bot:

```text
🕓 These appointment times are available on Thursday, 20 August.

Please choose a time.
```

Example:

```text
10:00 AM
11:30 AM
2:00 PM
4:00 PM
5:30 PM
```

Patient taps:

```text
4:00 PM
```

No manual date or time typing is required during the normal booking flow.

---

# 13. Availability Rules

For the MVP, availability will be maintained manually by the clinic in Google Sheets.

PhysioBot does not calculate complex staff schedules.

The `SLOTS` sheet may contain:

| Date | Time | Appointment Type | Provider Group | Available |
|---|---|---|---|---|
| 20 Aug | 10:00 AM | Clinic | Any | Yes |
| 20 Aug | 11:30 AM | Clinic | Female | Yes |
| 20 Aug | 2:00 PM | Home | Any | Yes |
| 20 Aug | 4:00 PM | Clinic | Female | Yes |

The structure may be simplified during implementation if appropriate.

### Provider Group

Possible values:

```text
Female
Male
Any
```

This allows PhysioBot to avoid showing obviously unsuitable slots when a patient requests a particular provider gender.

However:

**Selecting a slot does not guarantee provider assignment.**

Dr. Quratulain still makes the final decision.

---

# 14. Important Slot Rule

An available slot means:

> **Available to request**

It does not mean:

> **Automatically booked**

A patient's slot remains unconfirmed until the clinic approves the request.

This distinction should always be clear to the patient.

---

# 15. Reason for Visit

After slot selection:

```text
Briefly tell us what you would like help with.

For example:

• Back pain
• Knee pain
• Sports injury
• Post-surgery rehabilitation
```

Patient types a short response.

Example:

```text
Lower back pain for the last few weeks.
```

PhysioBot stores the response but does not:

- Diagnose
- Recommend treatment
- Interpret symptoms medically

---

# 16. Home Visit Address

Only shown when:

```text
Appointment Type = Home Visit
```

Bot:

```text
Please enter the address for the home visit.
```

Patient types the address.

For clinic visits, this question is skipped.

---

# 17. Booking Review

Before submission:

```text
Please review your appointment request:

Name: Sarah Ahmed
Type: Home Visit
Requested Date: 20 August
Requested Time: 4:00 PM
Preference: Female Physio
Reason: Lower back pain

Please remember that your appointment is not confirmed until the clinic reviews your request.
```

Buttons:

```text
Submit Request
Start Over
```

Individual-field editing is not required for the demo MVP.

---

# 18. Booking Creation

When the patient taps:

```text
Submit Request
```

n8n:

1. Generates an Appointment ID
2. Creates a row in the `Bookings` sheet
3. Stores the selected date/time
4. Sets status to `Pending`
5. Records the patient's WhatsApp number
6. Records creation timestamp
7. Clears temporary conversation state
8. Sends acknowledgement

---

# 19. Patient Acknowledgement

Example:

```text
✅ Appointment request received.

📅 Thursday, 20 August
🕓 4:00 PM
🏠 Home Visit
👩 Female Physiotherapist requested

Your appointment is not confirmed yet.

The clinic will review your request and send you confirmation shortly.
```

The phrase:

```text
Your appointment is not confirmed yet.
```

should always be included at this stage.

---

# 20. Google Sheet — Bookings Tab

Required columns:

| Column | Purpose |
|---|---|
| Appointment ID | Unique booking ID |
| Created At | Request timestamp |
| Patient Name | Patient |
| WhatsApp Number | Contact |
| Appointment Type | Clinic/Home |
| Requested Date | Selected date |
| Requested Time | Selected time |
| Provider Preference | Female/Male/None |
| Reason | Reason for visit |
| Address | Home visit address |
| Status | Pending/Confirmed/Rejected |
| Confirmed Date | Final appointment date |
| Confirmed Time | Final appointment time |
| Assigned Provider | Final physiotherapist |
| Assignment Notes | Internal notes |
| 24h Reminder Sent | Yes/No |
| 2h Reminder Sent | Yes/No |
| Confirmation Sent | Yes/No |
| Updated At | Last modification |

---

# 21. Booking Statuses

The MVP uses only:

```text
Pending
Confirmed
Rejected
```

## Pending

Patient submitted a request.

## Confirmed

Dr. Quratulain approved and assigned the appointment.

## Rejected

Clinic cannot accept the request.

There is no permanent `Rescheduled` status.

If a confirmed appointment changes, its status remains:

```text
Confirmed
```

and the patient receives an update notification.

---

# 22. Clinic Review Workflow

Dr. Quratulain opens the `Bookings` sheet.

Example pending request:

```text
Sarah Ahmed
Home Visit
20 August
4:00 PM
Female Physio
Lower back pain
Pending
```

She can then set:

```text
Confirmed Date
Confirmed Time
Assigned Provider
Assignment Notes
Status
```

For a straightforward request, the confirmed date/time can be the same as the requested slot.

---

# 23. Provider Assignment

Example dropdown:

```text
Dr. Quratulain
Dr. Mariya
Male Physio
```

Names should be configurable.

PhysioBot never assigns providers automatically.

---

# 24. Assignment Notes

Optional internal field.

Examples:

```text
Patient requested female physio.

Bring dry needling equipment.

First consultation.

Post-op knee rehab.
```

Assignment notes may be sent to the assigned physiotherapist.

They are never shown to the patient.

---

# 25. Confirmation Workflow

Dr. Quratulain sets:

```text
Confirmed Date = 20 August
Confirmed Time = 4:00 PM
Assigned Provider = Dr. Mariya
Status = Confirmed
```

n8n detects the update.

The automation then:

1. Sends patient confirmation
2. Sends provider assignment notification
3. Marks confirmation as sent

---

# 26. Patient Confirmation

Example:

```text
✅ Your physiotherapy appointment is confirmed.

📅 Thursday, 20 August
🕓 4:00 PM
👩‍⚕️ Physiotherapist: Dr. Mariya

If you need to make a change, please contact the clinic.
```

---

# 27. Provider Notification

Example:

```text
📋 New Appointment Assigned

Patient: Sarah Ahmed
Date: 20 August
Time: 4:00 PM
Type: Home Visit
Reason: Lower back pain

Address:
[Patient Address]

Notes:
Bring dry needling equipment.
```

Only information needed by the physiotherapist should be included.

---

# 28. Appointment Changes

Dr. Quratulain may change:

- Confirmed Date
- Confirmed Time
- Assigned Provider

n8n detects the change.

Patient receives:

```text
🔄 Your physiotherapy appointment has been updated.

📅 Friday, 21 August
🕓 3:00 PM
👩‍⚕️ Dr. Mariya
```

If the assigned provider changes, the appropriate provider notification should also be sent.

---

# 29. Rejection

If Dr. Quratulain changes:

```text
Status = Rejected
```

patient receives:

```text
We're sorry, but we are unable to confirm your requested appointment.

Please contact the clinic and we'll be happy to help you find another suitable time.
```

Custom rejection reasons are optional and not required for the first demo.

---

# 30. Automated Reminders

Only:

```text
Status = Confirmed
```

appointments receive reminders.

## Reminder 1

Approximately:

```text
24 hours before appointment
```

## Reminder 2

Approximately:

```text
2 hours before appointment
```

---

# 31. Reminder Example

```text
⏰ Appointment Reminder

Hi Sarah,

This is a reminder about your physiotherapy appointment.

📅 Thursday, 20 August
🕓 4:00 PM
👩‍⚕️ Dr. Mariya

Please contact the clinic if you need assistance.
```

Appropriate WhatsApp message templates should be used where required.

---

# 32. Duplicate Reminder Protection

Before sending:

```text
24h Reminder Sent?
```

If:

```text
No
```

send reminder and update:

```text
24h Reminder Sent = Yes
```

Same logic applies to:

```text
2h Reminder Sent
```

---

# 33. Workflow 1 — WhatsApp Intake

```text
WhatsApp Trigger
        ↓
Identify Patient
        ↓
Read Temporary State
        ↓
Welcome / Booking
        ↓
Collect Name
        ↓
Appointment Type
        ↓
Provider Preference
        ↓
Read SLOTS Sheet
        ↓
Show Available Dates
        ↓
Patient Selects Date
        ↓
Read Available Times
        ↓
Patient Selects Time
        ↓
Reason for Visit
        ↓
Home Visit?
   ┌────┴────┐
  Yes        No
   ↓          ↓
Address       │
   └────┬─────┘
        ↓
Show Booking Summary
        ↓
Submit
        ↓
Create Google Sheet Row
        ↓
Status = Pending
        ↓
Clear Temporary State
        ↓
Send Request Received
```

---

# 34. Workflow 2 — Clinic Decision & Notifications

```text
Google Sheets Trigger
        ↓
Booking Updated
        ↓
Check Status / Important Fields
        ↓
 ┌───────────┬───────────┬─────────────┐
 ↓           ↓           ↓
Confirmed   Rejected    Confirmed
                         booking changed
 ↓           ↓           ↓
Patient     Patient      Patient
Confirmation Message     Update
 ↓
Provider
Notification
```

---

# 35. Workflow 3 — Reminder Engine

```text
Schedule Trigger
Every 15 minutes
        ↓
Read Confirmed Appointments
        ↓
Calculate Time Until Appointment
        ↓
24h Reminder Due?
        ↓
Already Sent?
        ↓ NO
Send Reminder
        ↓
Mark Sent
```

Same logic for the 2-hour reminder.

---

# 36. Availability Management

For this MVP, clinic staff can manage availability directly from Google Sheets.

Example:

```text
Date        Time       Type       Provider Group    Available

20 Aug      10:00 AM   Clinic     Female            Yes
20 Aug      11:30 AM   Clinic     Any               Yes
20 Aug      2:00 PM    Home       Male              Yes
20 Aug      4:00 PM    Clinic     Female            No
```

Setting:

```text
Available = No
```

causes that slot to stop appearing to patients.

This provides a simple way to demonstrate dynamic availability without building a scheduling engine.

---

# 37. Slot Conflict Rule

The first MVP will **not** implement temporary slot reservation.

Therefore, two patients could theoretically request the same available slot before Dr. Quratulain reviews either request.

This is acceptable for the capability demo because:

- Requests are not automatically confirmed
- Dr. Quratulain makes the final decision
- The prototype is intended to validate the workflow

Automatic slot locking can be introduced later if the clinic wants it.

---

# 38. Basic Error Handling

Required:

- Failed Google Sheet writes visible in n8n
- Failed WhatsApp sends visible in n8n
- Appointment IDs generated uniquely
- Duplicate webhook processing should not intentionally create duplicate bookings
- Invalid or expired conversation state can restart the booking flow

Production-grade queue infrastructure is not required.

---

# 39. Explicitly Out of Scope

Do not build the following for this prototype:

- Custom admin dashboard
- Patient portal
- Payments
- Medical records
- Treatment notes
- Diagnosis
- AI medical advice
- Automatic provider assignment
- Automatic appointment confirmation
- Full provider scheduling engine
- Automatic slot locking
- Complex calendar calculations
- Google Calendar integration
- Route optimization
- Multi-branch scheduling
- Patient self-service rescheduling
- Analytics dashboard
- Payroll
- Inventory
- PostgreSQL
- NestJS
- BullMQ
- Formal audit subsystem

---

# 40. Possible Next Features

Only consider these after clinic feedback.

## Slot Reservation

Temporarily hold the patient's selected slot while the request is pending.

## Automatic Conflict Detection

Warn Dr. Quratulain if another appointment already occupies the requested slot.

## Provider-Specific Availability

Automatically calculate slots from each physiotherapist's working hours.

## Patient Reschedule Request

Add:

```text
Request Different Time
```

without allowing patients to directly change confirmed appointments.

## Cancellation

Allow:

```text
Request Cancellation
```

## FAQs

Automate common questions such as:

```text
Where is the clinic?

Do you offer home visits?

What are your working hours?

Do you treat sports injuries?
```

## AI Receptionist

Natural-language questions could be handled by AI while the appointment workflow remains structured.

## Calendar Integration

Confirmed bookings could automatically create calendar events.

---

# 41. Demo Success Criteria

The demo succeeds if Dr. Quratulain can see:

1. Patient starts on WhatsApp.
2. Patient taps `Book Appointment`.
3. Patient chooses Clinic/Home using buttons.
4. Patient chooses provider preference using buttons.
5. PhysioBot displays real available dates.
6. Patient taps a date.
7. PhysioBot displays only available times for that date.
8. Patient selects a slot without typing date/time.
9. Patient submits the request.
10. Booking automatically appears in Google Sheets as `Pending`.
11. Dr. Quratulain assigns the provider.
12. Dr. Quratulain confirms the appointment.
13. Patient automatically receives confirmation.
14. Assigned physiotherapist automatically receives appointment information.
15. Patient automatically receives a reminder.

---

# 42. Recommended Live Demo

### Patient

Starts WhatsApp conversation.

PhysioBot:

```text
👋 Welcome to [Clinic Name].

How can we help?
```

Patient taps:

```text
Book Appointment
```

Enters:

```text
Sarah Ahmed
```

Taps:

```text
Home Visit
```

Taps:

```text
Female Physio
```

PhysioBot:

```text
📅 Choose an available date:
```

Patient taps:

```text
Thursday, 20 August
```

PhysioBot:

```text
🕓 Available times:
```

Patient taps:

```text
4:00 PM
```

Patient enters:

```text
Lower back pain
```

Enters home address.

PhysioBot shows summary.

Patient taps:

```text
Submit Request
```

---

## Show Dr. Quratulain

Open Google Sheets immediately.

Show:

```text
Sarah Ahmed
Home Visit
20 August
4:00 PM
Female Physio
Lower back pain
Pending
```

Explain:

> The patient selected one of the appointment slots you made available. Nobody from the clinic had to collect or type any of this information.

Then Dr. Quratulain sets:

```text
Assigned Provider: Dr. Mariya
Confirmed Date: 20 August
Confirmed Time: 4:00 PM
Status: Confirmed
```

Patient automatically receives:

```text
✅ Your physiotherapy appointment is confirmed.

20 August
4:00 PM
Dr. Mariya
```

The test provider receives the assignment.

Then demonstrate an appointment reminder.

---

# 43. MVP Positioning

Present PhysioBot as:

> **An automated WhatsApp receptionist that lets patients request available appointment slots while the clinic keeps final control of scheduling.**

The value proposition is:

**Patients see available times immediately.**

**Less repetitive WhatsApp conversation.**

**No manual copying of patient details.**

**Automatic confirmations and reminders.**

**Dr. Quratulain still makes the final decision.**

---

# 44. Locked Technical Scope

Build only:

```text
WhatsApp Cloud API
+
n8n
+
n8n Data Tables
+
Google Sheets
```

Google Sheets contains:

```text
Bookings
SLOTS
```

Build only three primary n8n workflows:

```text
1. WhatsApp Intake & Available Slot Selection

2. Clinic Decision & Notifications

3. Appointment Reminders
```

---

# 45. Build Order

## Phase 1 — Foundation

- WhatsApp Cloud API connection
- n8n webhook/trigger
- Send and receive test messages
- Interactive buttons

## Phase 2 — Intake

- Temporary conversation state
- Name
- Clinic/Home Visit
- Provider preference
- Reason
- Conditional home address

## Phase 3 — Availability

- Use the existing `SLOTS` Google Sheet
- Read available dates
- Display WhatsApp date menu
- Read available times
- Display WhatsApp time menu
- Store selected slot

## Phase 4 — Booking Creation

- Booking summary
- Submit button
- Generate Appointment ID
- Create `Pending` booking in Google Sheets
- Send acknowledgement

## Phase 5 — Clinic Decision

- Google Sheets dropdowns
- Assign provider
- Confirm/reject
- Detect changes with n8n
- Patient notification
- Provider notification

## Phase 6 — Reminders

- 24-hour reminder
- 2-hour reminder
- Duplicate-send protection

## Phase 7 — Demo Polish

- Friendly wording
- Emoji/icon consistency
- Error states
- Restart flow
- Test multiple booking scenarios

---

# 46. Final MVP Rule

Before adding any feature, ask:

> **Does this make the WhatsApp automation capability more convincing to Dr. Quratulain without significantly increasing complexity?**

If yes:

Consider it.

If no:

**Do not build it yet.**
