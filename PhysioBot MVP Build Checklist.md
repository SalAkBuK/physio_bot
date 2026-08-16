# PhysioBot MVP Build Checklist

## Phase 0 – Environment Setup

### n8n

- [ 1 ] Install n8n
- [ 1 ] Launch n8n locally
- [ 1 ] Create admin account
- [ 1 ] Verify workflows can be created and executed

### Google

- [ 1 ] Create MVP Google Sheet
- [ 1 ] Create Google Cloud project
- [ 1 ] Enable Google Sheets API
- [ 1 ] Create service account
- [ 1 ] Generate credentials
- [ 1 ] Connect Google Sheets to n8n
- [ 1 ] Verify row creation from n8n

### WhatsApp

- [ ] Create Meta Developer account
- [ ] Create WhatsApp Cloud API app
- [ ] Obtain test phone number
- [ ] Obtain access token
- [ ] Verify ability to send test messages

---

# Phase 1 – Booking Intake MVP

Goal:

Patient booking request appears automatically in Google Sheets.

## Sheet Design

- [ ] Create appointment sheet
- [ ] Add Appointment ID column
- [ ] Add Status column
- [ ] Add Date column
- [ ] Add Time column
- [ ] Add Patient Name column
- [ ] Add Phone column
- [ ] Add Gender column
- [ ] Add Appointment Type column
- [ ] Add Provider Preference column
- [ ] Add Service Type column
- [ ] Add Address column
- [ ] Add Assigned Provider column
- [ ] Add Assignment Notes column
- [ ] Add Created At column
- [ ] Add Updated At column

## Intake Workflow

- [ ] Create workflow
- [ ] Accept booking data
- [ ] Validate required fields
- [ ] Generate Appointment ID
- [ ] Set Status = Pending
- [ ] Add Created At timestamp
- [ ] Add Updated At timestamp
- [ ] Append booking to Google Sheet
- [ ] Handle Google Sheet failure
- [ ] Log workflow execution

## Confirmation

- [ ] Send booking received confirmation
- [ ] Test successful booking flow
- [ ] Test invalid booking flow

### Phase 1 Complete When

- [ ] Booking request reaches Google Sheet
- [ ] Status is Pending
- [ ] Confirmation message is sent

---

# Phase 2 – WhatsApp Conversation Flow

Goal:

Collect booking information through WhatsApp.

## Conversation Design

- [ ] Define greeting message
- [ ] Define booking start message
- [ ] Define name collection step
- [ ] Define gender collection step
- [ ] Define appointment type step
- [ ] Define date collection step
- [ ] Define time collection step
- [ ] Define provider preference step
- [ ] Define service type step
- [ ] Define address step
- [ ] Define notes step
- [ ] Define booking summary message

## Workflow

- [ ] Connect WhatsApp webhook
- [ ] Receive incoming messages
- [ ] Store conversation state
- [ ] Collect all required fields
- [ ] Submit completed booking
- [ ] Create Pending booking
- [ ] Send booking received confirmation

### Phase 2 Complete When

- [ ] Patient can complete booking entirely through WhatsApp

---

# Phase 3 – Review & Provider Assignment

Goal:

Allow Dr. Quratulain to manage requests.

## Assignment Workflow

- [ ] Define review process
- [ ] Define approval process
- [ ] Define rejection process
- [ ] Define reschedule process
- [ ] Define provider assignment process

## Google Sheet Updates

- [ ] Update Assigned Provider
- [ ] Update Assignment Notes
- [ ] Update Status
- [ ] Update Updated At timestamp

## Notifications

- [ ] Send confirmation notification
- [ ] Send rejection notification
- [ ] Send reschedule notification
- [ ] Send provider assignment notification

### Phase 3 Complete When

- [ ] Provider assignment works
- [ ] Status updates work
- [ ] Notifications are delivered

---

# Phase 4 – Automated Reminders

Goal:

Reduce missed appointments.

## Reminder Workflow

- [ ] Identify upcoming appointments
- [ ] Send 24-hour reminder
- [ ] Send 2-hour reminder
- [ ] Prevent duplicate reminders
- [ ] Retry failed sends

## Testing

- [ ] Test 24-hour reminder
- [ ] Test 2-hour reminder
- [ ] Test failed message handling

### Phase 4 Complete When

- [ ] Reminder automation is reliable

---

# Phase 5 – Hardening

Goal:

Make MVP usable in a real clinic.

## Reliability

- [ ] Add retry handling
- [ ] Add workflow error logging
- [ ] Add Google API failure handling
- [ ] Add WhatsApp API failure handling

## Audit Trail

- [ ] Log booking creation
- [ ] Log assignment
- [ ] Log rejection
- [ ] Log reschedule
- [ ] Log notification failures

## Documentation

- [ ] Update PROJECT\_CONTEXT.md
- [ ] Document workflow architecture
- [ ] Document setup process
- [ ] Document recovery procedures

### MVP Launch Ready When

- [ ] Intake automation works
- [ ] Google Sheet sync works
- [ ] Assignment workflow works
- [ ] Notifications work
- [ ] Reminders work
- [ ] End-to-end booking flow tested
