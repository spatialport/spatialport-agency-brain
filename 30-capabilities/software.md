---
id: sp-capability-software
client_id: spatial-port
record_type: knowledge
service_path: software
status: proposed
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
# Software Capability

Custom software used to serve clients; Spatial Port owns it unless the manifest states an exception.

## Definition of done

- Inputs are explicit.
- Output and owner are explicit.
- Review type is explicit.
- Source files and final deliverables are registered.
- Learning is proposed separately from execution.

## Track record & references (proposed)

- **NXTO × Spatialport OS** — the flagship internal build, live at os.spatial-port.io: Next.js 15 dashboard, AWS DynamoDB + Lambda backend, real auth (scrypt + JWT + TOTP, zero-dependency), directive orchestration with atomic claims and server-side audit log, hourly agent heartbeat (NXTO CLAUDE.md; SPRINTS.md Sprints 0–13).
- **Thi Land** — "sistema punti & accessi" (points and access system), in build 2026-05→2026-07 (projects.ts phase tl-3, task t-tl-1).
- **Farmacia Vitalis** — vitalis-mail backend (conversation record 2026-08).
- **Lead engine** — Lambda `spatialport-submit-lead` + DynamoDB `spatialport-leads` + SES notifications behind spatial-port.com (NXTO CLAUDE.md pointers).
- **Deploy tooling** — idempotent AWS static-site provisioning reused across all `*.spatial-port.io` demos and the dashboard (`client-demos/*/setup-domain.sh`; see 70-sanitized-learnings/2026-08-19__deploy-pattern-aws-subdomains.md).
- **Marmareos AI tools** — AI preventivi + CRM intelligence (client-facing software line in progress, projects.ts phase mm-4).
