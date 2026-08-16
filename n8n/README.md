# n8n Workflows

Exported n8n workflow JSON files are version-controlled in `workflows/`. Credentials and secret values remain in the configured n8n instance and must never be committed.

The preserved `00-google-sheets-slot-poc.json` is historical proof-of-concept code. It uses hardcoded test inputs and marks a selected slot `Booked`; that behavior must not be copied into production intake, where a patient selection creates only a `Pending` request.

## Development and handoff

```text
Codex changes workflow JSON in Git
        ↓
Changes are fetched on the laptop containing the configured n8n instance
        ↓
Workflow is imported or updated
        ↓
Existing local credential references are selected or reused
        ↓
User executes the acceptance test
```

Workflows should be added only when their implementation iteration begins. Do not create empty JSON placeholders. Validate edited JSON before committing, and do not attempt to configure Google OAuth from this repository.

**Secrets must never be committed.**
