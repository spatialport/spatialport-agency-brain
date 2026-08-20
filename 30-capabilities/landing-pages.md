---
id: sp-capability-landing-pages
client_id: spatial-port
record_type: knowledge
service_path: landing-pages
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
# Landing Pages Capability

Landing-page strategy, copy, design, build, QA and publishing.

## Definition of done

- Inputs are explicit.
- Output and owner are explicit.
- Review type is explicit.
- Source files and final deliverables are registered.
- Learning is proposed separately from execution.

## Track record & references (proposed)

- **Thi Land** — waitlist landing shipped and converted to live ticket sales (NXTO projects.ts task t-tl-4 "Landing waitlist → biglietti live", phase tl-2).
- **spatial-port.com v2** — bilingual (EN+IT) cinematic site with its own serverless lead engine (Lambda `spatialport-submit-lead` + DynamoDB + SES) (projects.ts sito-spatial-port; NXTO CLAUDE.md pointers).
- **Demo/sales landing system** — Great Bear Vineyards (v1+v2), Nizza Milano, Ron Mann Design, Scott sales kit, each live on its own `*.spatial-port.io` subdomain with idempotent AWS deploy scripts (`SITO SPATIALPORT/client-demos/`). Proven pre-sales asset: a polished multi-page demo shipped before the contract.
- **Elvetica Locker** (Onniversum CH) — site restyling + domain migration with SSL, redirects and QA, quoted at ~2–3 working days turnaround (Preventivo-Onniversum-Elvetica-Locker.md).
- Reusable method: see 70-sanitized-learnings/2026-08-19__deploy-pattern-aws-subdomains.md.
