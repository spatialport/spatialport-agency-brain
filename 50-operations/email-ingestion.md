---
id: sp-operation-email-ingestion
client_id: spatial-port
record_type: policy
service_path: company
status: accepted
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: internal
source_ref: manual://implementation-handoff
schema_version: 1.0.0
created_at: 2026-08-11
updated_at: 2026-08-11
---
# Email Ingestion

1. Ingest only labeled client/project threads, never whole mailboxes by default.
2. Keep raw email internal and outside the client window.
3. Extract explicit feedback, commitments, approvals and task proposals.
4. Link to the source message ID; do not copy signatures, tokens or unrelated history.
