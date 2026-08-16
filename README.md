# PhysioBot

PhysioBot is an n8n-based WhatsApp appointment automation MVP for a physiotherapy clinic. It reduces repetitive appointment intake while keeping final scheduling and physiotherapist assignment under clinic control.

## MVP architecture

```text
WhatsApp Cloud API
        ↓
       n8n
        ↓
 n8n Data Tables + Google Sheets
 temporary state     Bookings and SLOTS
        ↓
WhatsApp notifications
```

No custom backend or administrative dashboard is required. Google Sheets remains the MVP operational source of truth.

## Repository layout

```text
docs/
  PROJECT_CONTEXT.md  Locked business and technical scope
  ITERATIONS.md       Incremental development sequence
  TESTING.md          Implementation tasks and acceptance criteria
  CURRENT_STATE.md    Evidence-based status and next iteration
n8n/
  README.md           Workflow development and handoff guidance
  workflows/          Version-controlled n8n exports
AGENTS.md              Rules for future repository work
```

## Current state

Iteration 0 preserves an inactive Google Sheets slot-booking proof of concept and establishes the repository structure. Runtime integration tests are performed later on the separate n8n installation where credentials are already configured.

Development proceeds one requested iteration at a time. See [project context](docs/PROJECT_CONTEXT.md), [current state](docs/CURRENT_STATE.md), and [testing criteria](docs/TESTING.md). The next planned step is Iteration 1 — Dynamic Availability; it is not implemented yet.
