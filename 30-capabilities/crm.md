---
id: sp-capability-crm
client_id: spatial-port
record_type: knowledge
service_path: crm
status: accepted
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: internal
source_ref: manual://2026-08-19-knowledge-sync
schema_version: 1.1.0
created_at: 2026-08-11
updated_at: 2026-08-19
---
# Crm Capability

Lead routing, lifecycle definitions, reporting and future CRM integration.

## Definition of done

- Inputs are explicit.
- Output and owner are explicit.
- Review type is explicit.
- Source files and final deliverables are registered.
- Learning is proposed separately from execution.

## Track record & references (proposed)

- **Spatial Port's own pipeline (dogfooding):** lead engine on spatial-port.com → DynamoDB `spatialport-leads` → NXTO /pipeline with deal kanban (Nuovo→Contattato→In trattativa→Vinto/Perso), per-lead value, CHF/EUR split, and a 1–5 qualification panel persisted on each lead (NXTO CLAUDE.md; SPRINTS.md T-041/T-042).
- **Marmareos** — AI quoting tool ("AI preventivi") v1 in build, plus call-transcription + CRM-intelligence scoping — the reference for client-facing CRM/AI work (projects.ts tasks t-mm-2, t-mm-4, phase mm-4).
- Operations: Hunter qualifies inbound leads and drafts follow-ups/quotes; never emails prospects without Alex's approval (NXTO agents.ts, Hunter limits). CRM Administrator and Customer Success specialists sit under Hunter (agents.ts specialists).
