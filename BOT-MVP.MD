# PhysioBot MVP – Project Context

Version: 1.0
Last Updated: 2026-08-16
Project Status: Planning / MVP Design

# Physiotherapy WhatsApp Booking System — MVP Requirements (Locked v1)

## Goal

Reduce administrative workload by automating appointment intake, confirmations, and reminders while keeping scheduling and provider assignment under Dr. Quratulain's control.

The system is **not** intended to replace current scheduling processes.

The system is intended to:

* Collect patient information automatically
* Reduce repetitive WhatsApp conversations
* Centralize booking requests
* Keep provider assignment manual
* Synchronize with the clinic's existing Google Sheet workflow
* Send appointment reminders automatically

---

# Primary Users

## 1. Patient

Can:

* Request appointments
* Provide booking details
* Receive confirmations
* Receive reminders
* Receive rescheduling notifications

Cannot:

* Self-assign providers
* Modify confirmed appointments without contacting clinic

---

## 2. Dr. Quratulain (Scheduler)

Can:

* Review booking requests
* Approve requests
* Reject requests
* Reschedule requests
* Assign providers
* Add assignment notes
* View booking queue

Acts as the final decision maker.

---

## 3. Physiotherapist

Can:

* Receive assigned appointments
* View appointment details
* Receive scheduling updates

Cannot:

* Reassign appointments
* Modify clinic schedule

---

# Core Workflow

```text
Patient
    ↓
WhatsApp Bot
    ↓
Collect Information
    ↓
Booking Request Created
    ↓
Pending Queue
    ↓
Dr. Quratulain Reviews
    ↓
Approve / Reject / Reschedule
    ↓
Assign Provider
    ↓
Google Sheet Updated
    ↓
Patient Confirmation Sent
    ↓
Provider Notification Sent
    ↓
Automated Reminders
```

---

# Patient Intake Flow

Patient initiates:

```text
Book Appointment
```

Bot collects:

## Required Fields

### Patient Name

```text
Full Name
```

### Phone Number

```text
Mobile Number
```

Use WhatsApp number by default if available.

---

### Gender

Options:

```text
Male
Female
```

---

### Appointment Type

Options:

```text
Clinic Visit
Home Visit
```

---

### Preferred Date

Example:

```text
20 August 2026
```

---

### Preferred Time

Example:

```text
4:00 PM
```

---

### Provider Preference

Options:

```text
Female Physiotherapist
Male Physiotherapist
No Preference
```

---

### Service Type

Options:

```text
General Physiotherapy
Sports Rehabilitation
Post-Operative Rehabilitation
Dry Needling
Home Visit Treatment
Other
```

---

### Address

Required only if:

```text
Appointment Type = Home Visit
```

---

### Notes

Optional field.

Example:

```text
Neck pain
Back pain
Post surgery recovery
```

---

# Booking Request Statuses

## Pending

Created after intake.

Waiting for review.

---

## Confirmed

Approved and assigned.

---

## Rescheduled

Appointment moved.

---

## Rejected

Booking declined.

---

## Completed

Future phase.

Not part of MVP workflow.

---

# Booking Queue

Dr. Quratulain sees:

```text
Patient Name
Gender
Phone
Appointment Type
Preferred Date
Preferred Time
Provider Preference
Service Type
Address
Notes
Status
```

---

# Review Actions

## Approve & Assign

Assign provider.

---

## Reschedule

Propose alternative date/time.

---

## Reject

Decline booking.

Optional rejection note.

---

# Provider Assignment

Available assignment options:

```text
Dr. Quratulain
Dr. Mariya
Male Physio
```

Provider list must be configurable.

No hardcoded names.

---

## Assignment Notes

Optional internal notes.

Examples:

```text
Patient requested female physio.

Bring dry needling kit.

First consultation.

Post-op knee rehab.
```

Notes visible to assigned provider.

---

# Scheduling Rules

## Human Decision Required

System never automatically assigns providers.

System never automatically confirms appointments.

Dr. Quratulain remains final authority.

---

## Availability Warning

System checks existing appointments.

If conflict detected:

```text
Warning:
Potential scheduling conflict.
```

Examples:

* Same provider booked
* Duplicate time slot
* Overlapping home visit

System warns only.

Does not block assignment.

---

# Google Sheet Integration

## Purpose

Google Sheet remains scheduling source of truth during MVP.

---

## Required Columns

| Field               | Description           |
| ------------------- | --------------------- |
| Appointment ID      | Unique identifier     |
| Status              | Pending/Confirmed/etc |
| Date                | Appointment date      |
| Time                | Appointment time      |
| Patient Name        | Patient               |
| Phone               | Contact               |
| Gender              | Gender                |
| Appointment Type    | Clinic/Home           |
| Provider Preference | Preference            |
| Service Type        | Treatment category    |
| Address             | Home visit address    |
| Assigned Provider   | Assigned physio       |
| Assignment Notes    | Internal notes        |
| Created At          | Request timestamp     |
| Updated At          | Last modification     |

---

# Notifications

## Patient Confirmation

Sent when appointment is confirmed.

Example:

```text
Your appointment has been confirmed.

Date: 20 August
Time: 4:00 PM
Provider: Dr. Mariya
```

---

## Provider Notification

Sent when appointment assigned.

Contains:

* Patient details
* Appointment details
* Notes

---

## Reschedule Notification

Sent whenever appointment changes.

---

## Rejection Notification

Sent when booking rejected.

---

# Automated Reminders

## Reminder 1

24 hours before appointment.

---

## Reminder 2

2 hours before appointment.

---

## Reminder Content

Include:

* Patient name
* Appointment date
* Appointment time
* Assigned provider

---

# Audit Requirements

Track:

* Request creation
* Approval
* Rejection
* Reschedule
* Provider assignment
* Sheet update failures

Store:

```text
Timestamp
Action
Actor
Booking ID
```

---

# Non-Functional Requirements

## Simplicity

System must require minimal training.

---

## Mobile Friendly

Administrative actions should be usable from a phone.

---

## Reliability

No booking request may be lost if:

* WhatsApp API temporarily fails
* Google Sheets API temporarily fails

Requests must be retried.

---

## Performance

Booking creation should complete within a few seconds.

---

# Explicitly Out of Scope (Not MVP)

* Payments
* Revenue tracking
* Patient medical records
* Treatment notes
* Calendar integrations
* Route optimization
* Multi-branch management
* Analytics dashboards
* Automatic provider assignment
* AI scheduling recommendations
* Patient self-service rescheduling
* Staff payroll
* Inventory management

---

# MVP Success Criteria

The MVP is successful if:

1. Patients can request appointments entirely through WhatsApp.
2. Dr. Quratulain no longer manually collects booking information.
3. Provider assignment remains under clinic control.
4. Confirmed appointments automatically appear in the shared Google Sheet.
5. Patients automatically receive confirmations and reminders.
6. Physiotherapists automatically receive assignment notifications.
7. Existing clinic workflow continues with minimal change.

## Key Decisions

### D-001
Google Sheets is the operational source of truth during MVP.

Reason:
Fast adoption and minimal workflow change.

### D-002
Provider assignment remains manual.

Reason:
Clinic requires human control.

### D-003
WhatsApp is the primary intake channel.

Reason:
Matches current patient behavior.

## Open Questions

- How many appointments are booked per day?
- Is Google Sheets currently used by all staff?
- Are home visits a significant percentage of bookings?
- Is provider preference mandatory or optional?
- What WhatsApp message templates are preferred?

## MVP Success Metrics

- 80%+ of new appointment requests captured through WhatsApp flow
- 100% of bookings written to Google Sheets
- Reminder delivery success rate > 95%
- Manual data entry reduced by at least 50%

# Architecture Status

Business requirements are locked.

Technical implementation is NOT locked.

The MVP should prioritize:

1. Fast validation
2. Low operational complexity
3. Minimal workflow disruption

Technology choices may change as long as the business requirements remain satisfied.

---

# Preferred MVP Architecture

Current preferred implementation:

```text
WhatsApp Cloud API
        ↓
       n8n
        ↓
Google Sheets
```

Responsibilities:

- Patient intake
- Data validation
- Booking creation
- Status updates
- Notifications
- Reminder scheduling

Goal:

Validate clinic workflow before building custom software.

---

# Future Architecture (Post-MVP)

Potential stack:

Backend:
- NestJS

Database:
- PostgreSQL

Queueing:
- BullMQ

Future architecture should only be introduced when:

- Workflow has been validated
- Operational volume increases
- Google Sheets becomes a bottleneck
- Advanced scheduling features become necessary

---

# Technical Principles

## Source of Truth

During MVP:

```text
Google Sheets
```

is the operational source of truth.

All booking records must ultimately exist in Google Sheets.

---

## Provider Assignment

System never automatically assigns providers.

System never automatically confirms appointments.

Human approval remains mandatory.

---

## Workflow First

Business workflow is more important than implementation details.

The clinic experience should remain unchanged even if the internal technology stack changes later.

---

# Phase 1 – Intake Automation

Objective:

Create a complete intake flow from WhatsApp into Google Sheets.

Flow:

```text
Patient
    ↓
WhatsApp
    ↓
n8n Workflow
    ↓
Google Sheets
    ↓
Confirmation Message
```

Responsibilities:

1. Collect required patient information
2. Validate required fields
3. Generate Appointment ID
4. Create booking request
5. Append row to Google Sheet
6. Send booking received confirmation

Success Criteria:

- Appointment appears in Google Sheets
- Booking status is Pending
- Patient receives confirmation
- No manual data entry required

---

# Phase 2 – Review & Assignment

Objective:

Support review and provider assignment workflow.

Capabilities:

- Approve booking
- Reject booking
- Reschedule booking
- Assign provider
- Update Google Sheet
- Send notifications

---

# Phase 3 – Automated Reminders

Objective:

Reduce missed appointments.

Capabilities:

- Reminder 24 hours before appointment
- Reminder 2 hours before appointment
- Retry failed sends

---

# Migration Principle

The MVP must not create technical lock-in.

Workflows should be designed so they can later move from:

```text
WhatsApp
    ↓
n8n
    ↓
Google Sheets
```

to:

```text
WhatsApp
    ↓
Backend
    ↓
PostgreSQL
    ↓
Google Sheets
```

without changing clinic operations.

---

# Repository Structure

docs/
  PROJECT_CONTEXT.md

n8n/
  workflows/

scripts/

infrastructure/
```