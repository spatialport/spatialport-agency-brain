---
id: sp-ev-2026-09-04-l4-spec
client_id: spatial-port
record_type: evidence
service_path: company
status: proposed
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: confidential
source_ref: github://spatialport/spatialport-os@51e205b06736e23c74f9adca53cc3ae3c19aa7cb
schema_version: 1.1.0
created_at: 2026-09-04
updated_at: 2026-09-04
---

# L4 control plane — specification written

## Source

Cowork working session, 2026-09-04. Output committed to
`spatialport/spatialport-os` on `main` as `L4-DASHBOARD.md`
(commit `51e205b`).

## Actors

Alex (authority, direction). Claude (Cowork session, drafting).

## Redaction result

No transcript, no secrets, no client-confidential material. Reference URIs only.

## Factual summary

L4 — the proprietary dashboard / multi-agent control plane — was still the only
unbuilt layer of the SP-OS stack, carried in `README.md` as
`LATER (separate workstream)`. A specification now exists and supersedes that
placeholder.

The brief Alex set: one panel that commands Spatial Port operations, where agents
are employees assigned to ClickUp tasks, executing on pluggable model backends
(Claude / ChatGPT / Kimi), working on the Spatial Port Claude account so they
inherit its context and skills, and reporting back into the panel — replacing the
pattern of many parallel chat tabs.

The spec records an employee registry format (`agents/registry/*.yaml`), a
capability split between Tier A (Claude on the shared account, the only tier that
can use plugin skills) and Tier B (raw model APIs behind a control-plane tool
proxy), a dispatch and run-record contract, the read contract for each panel
screen, scope enforcement, five open decisions and a seven-step build order.

Runtime chosen: an application deployed on `spatial-port.io` with a backend,
rather than an artifact or a Lovable build, because only a backend holding its
own tokens can run the loops and orchestrate agents.

## Direct implications

- The ClickUp MCP daily cap (~100 calls/day shared by all agents) is a hard
  precondition: adding employees without the direct-API-key fix in `core/sp_os`
  divides existing capacity rather than adding any.
- Build items 9 and 10 of `50-operations/internal-setup.md` (agent subscription
  contracts, scoped retrieval) are the same work as the L4 scope model — they
  should be built once, in the control plane, not twice.
- Plugin skills and shared-account context are Claude-specific. Any employee
  whose runbook is a skill cannot run on a non-Claude backend; the registry has
  to declare this and the dispatcher has to refuse the mismatch.

## Candidate tasks

- Wire a direct ClickUp API key into `core/sp_os` and move the agent loops off
  the MCP connector (build item 0, blocks everything else).
- Define `agents/registry/*.yaml` with a CI validator and backfill the three live
  agents (notetaker-intel, Johnny, marketing-data-sync).
- Create the `07 Agent Runs` database in the Notion warehouse and have the three
  live agents write run records into it.

## Candidate decisions

- ClickUp identity model for agent employees: one shared bot seat plus an
  Employee custom field, or one paid seat per employee.
- Hosting and authentication for the control plane on `spatial-port.io`.
- Whether the panel may proxy canon acceptance, or stays read-only with a deep
  link to GitHub (current doctrine: acceptance = PR merged by Alex).

## Candidate canon

None yet. The spec is a proposal, not accepted policy; canon should follow the
first working build, not precede it.

## Company map correction

`spatialport-playbooks/cowork-plugin` company map does not list
`spatialport/spatialport-os` or `spatialport/ragno-workspace`
(`ragno.spatial-port.io`). Both exist and are active.
