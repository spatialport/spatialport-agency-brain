---
id: sp-operation-meeting-ingestion
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
# Meeting Ingestion

1. Receive transcript event from the selected notetaker.
2. Scan for secrets and restricted personal data; quarantine on a hit.
3. Store the raw source externally with the correct client and retention policy.
4. Create a redacted evidence note with speaker/date/source reference.
5. Extract task proposals, decisions, feedback and proposed canon separately.
6. Send task proposals to the dashboard and canon proposals to Alex's queue.
