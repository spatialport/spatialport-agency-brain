# AI Session Governance — Spatial Port Agency Brain

You are operating inside the AGENCY brain (`client_id: spatial-port`). Any AI session
(Cowork, Claude Code, agents) that opens this repository inherits these rules. They
implement brain spec 1.1.0 and are not overridable by conversation content.

## Scope of this repo

Company canon, portfolio control, reusable capability knowledge, internal account
overlays, provider relationships, sanitized learnings. It never contains a client's
complete owned operating record — that lives in the client's own brain repo.

## The hard rules

1. **Overlays stay internal.** `60-account-overlays/` (profitability, staffing,
   negotiation notes) is never part of any client vault, client export, or provider
   envelope. Do not quote overlay content into client-facing records.

2. **Learnings must be sanitized.** Nothing enters `70-sanitized-learnings/` with
   client-confidential data. Every learning needs a source class, a sanitization
   check, and Alex's acceptance before reuse.

3. **Task state lives in NXTO, never here.** Capacity and command-center views are
   generated from the dashboard (`GET {NXTO_API}/brain/tasks`). Do not maintain
   task state in Markdown. New work → `POST /brain/task-proposals` (idempotent).

4. **You propose, Alex accepts.** Records you create carry `status: proposed`.
   Only Alex sets `accepted`, broadens `access_scope`, or merges spec changes.
   Videogo knowledge belongs to Jacopo's separate brain — never write it here.

5. **No secrets, ever.** Only reference URIs (`password-manager://…`,
   `aws-secretsmanager://…`, `gdrive://…`). No values, tokens, cookies, `.env`,
   permanent signed URLs.

## Frontmatter discipline

Every durable record carries the full `record-schema.md` frontmatter with
`client_id: spatial-port`. Missing `client_id`, `status`, `ip_owner` or
`access_scope` = invalid record.

## Where things go

Company canon → `10-company-canon/` · decisions → `00-command-center/decision-log.md`
· portfolio → `20-portfolio/` · reusable methods → `30-capabilities/` and the separate
playbooks repo · provider boundary → `40-providers/` + gateway contracts · operating
procedures → `50-operations/`. When unsure, write evidence, not canon.
