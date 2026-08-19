---
id: sp-operation-client-export-and-offboarding
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
# Client Export And Offboarding

1. Freeze new writes and resolve open approvals.
2. Export the complete client repository and client-owned Drive assets.
3. Exclude Spatial Port overlays, reusable playbooks, software and Videogo technology.
4. Revoke client/project groups, service identities, signed URLs and provider subscriptions.
5. Rotate only credentials actually exposed or shared; routine offboarding should rely on revocation.
6. Emit `client.offboarded` and preserve the legally required audit record.
