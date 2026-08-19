---
id: sp-decision-log
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
schema_version: 1.1.0
created_at: 2026-08-11
updated_at: 2026-08-19
---
# Decision Log

| Decision ID | Date | Decision | Owner | Why | Revisit trigger |
|---|---|---|---|---|---|
| DEC-001 | 2026-08-11 | One private repository per client | Alex | Creates a hard permission and export boundary | Revisit only if repository count becomes an operational bottleneck after automation |
| DEC-002 | 2026-08-11 | Dashboard owns task state | Alex | Avoids two competing task systems | Revisit only if dashboard is retired |
| DEC-003 | 2026-08-11 | Videogo connects only through task envelopes | Alex / Jacopo | Preserves independent IP and prevents cross-client access | Revisit only through both companies' signed agreement |
| DEC-004 | 2026-08-19 | Governance boundary: NXTO = runtime authority (task state, operational approvals of deliveries/spend, agent execution); Brains = knowledge authority (canon lifecycle proposed->accepted via PR merged only by Alex, IP/access policy, export boundary). NXTO surfaces pending canon PRs but never owns canon; brains never own live task status | Alex | Removes the only overlap between the two systems; each approval has exactly one home | Revisit if a second human authority is delegated canon acceptance |
| DEC-005 | 2026-08-19 | Event router = EventBridge bus `spatialport-events` (us-west-2) + audit log + 90-day replay archive + DLQ; `nxto-api` is the first emitter (task.created/updated, approval.completed) | Alex | One shared, replayable event backbone as required by the spec | Revisit only on multi-region needs |
| DEC-006 | 2026-08-19 | Canonical names frozen: company `Spatial Port` / `spatial-port`; client IDs `marmareos`, `thi-land`, `farmacia-vitalis`; provider `Videogo` confirmed; repos named `spatialport-*` | Alex | IDs must match NXTO project slugs before repos are created; no piecemeal renames later | Never (breaking change requires a new migration) |
