---
id: sp-ev-2026-09-04-l4-build
client_id: spatial-port
record_type: evidence
service_path: company
status: proposed
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: confidential
source_ref: github://spatialport/spatialport-os/pull/1
schema_version: 1.1.0
created_at: 2026-09-04
updated_at: 2026-09-04
---

# L4 control plane — first build

## Source

Cowork working session, 2026-09-04 (evening), following the spec session of
the same day (`sp-ev-2026-09-04-l4-spec`). Output: pull request
`spatialport/spatialport-os#1`, branch `l4/dashboard-v1`, 28 files.

## Actors

Alex (authority, organisation model, direction). Claude (Cowork session,
build).

## Redaction result

No transcript, no secrets, no client-confidential content. Demo data in the
build is plausible example work, labelled as such, not ClickUp state.

## Factual summary

Alex re-cut the L4 model from "dashboard over agents" to an **organisation**:
a CEO agent that routes and answers, a Chief of Staff agent as the jolly for
bottlenecks, hybrid and internal work, and one **Head of `<client>`** per
tenant that executes every vertical for that client, pulling from and pushing
to the client's brain. The backend of an employee is an API call to a model
plus an intelligence layer; Heads-of may run on Claude, Codex or Kimi; only
CEO and Chief of Staff must be Claude with the Spatial Port skills. Agents are
autonomous by default (no approval queue) and controllable on demand.

Structure decision taken in the build: the organisational unit is the
**client**, not the vertical; verticals are capabilities of a Head-of. Reason:
one-tenant-per-session is a hard rule and knowledge, contracts and ClickUp
spaces are already per client.

What shipped in the PR: spec v0.2 (`L4-DASHBOARD.md`), an employee registry
with seven YAML entries and a validator, three operating prompts, a Python
backend (`backend/sp_os_l4`: intelligence layer, Composio tool plane with
scope enforcement, dispatcher with autonomy gates, Anthropic and
OpenAI-compatible backends, run records, FastAPI with a mock mode, nine
passing tests) and a single-file dashboard (`dashboard/index.html`) in the
Spatial Port identity with Ask bar, Floor, Board (client × vertical), Runs,
Controlli and Health. A preview of the dashboard on demo data was published
as a claude.ai artifact.

## Direct implications

- Live runs remain blocked on build item 0: a direct ClickUp API token in
  `core/sp_os` so the seven employees do not share the ~100 calls/day MCP
  quota.
- The Composio tool plane makes tools model-agnostic; the only thing tier A
  adds is the skills in context. A Kimi-backed Head-of can read the brain and
  write ClickUp comments under the same scopes as a Claude-backed one.
- The company map in the `spatialport-brain` plugin now has a second consumer
  (`backend/sp_os_l4/intelligence.py` mirrors it) and must be kept in sync.

## Candidate tasks

- Wire `CLICKUP_API_TOKEN` into `core/sp_os` and route L4 dispatch through it.
- Create the `07 Agent Runs` database in the Notion warehouse with the
  properties documented in `backend/sp_os_l4/store.py`.
- Deploy the backend and the static dashboard on `spatial-port.io` behind auth.
- Add `spatialport-os` and `ragno-workspace` to the plugin company map.

## Candidate decisions

- ClickUp identity per employee (shared Johnny seat vs one seat each).
- Hosting and authentication for the control plane.
- Whether the panel may proxy canon acceptance.
- Cost ceiling per employee per day.
- Which Heads-of go live first (recommended: Farmacia Vitalis, Marmareos).

## Candidate canon

None yet; the organisation model becomes canon after the first live Head-of
proves it on real tasks.
