---
id: sp-ops-internal-setup
client_id: spatial-port
record_type: policy
service_path: company
status: proposed
owner: alex-bellesia
authority: alex-bellesia
ip_owner: spatial-port
access_scope: internal
sensitivity: confidential
source_ref: manual://ops-design-session-2026-08-20
schema_version: 1.1.0
created_at: 2026-08-20
updated_at: 2026-08-20
---
# Spatial Port — Internal Operating Setup

> **Status update 2026-08-20 (facts that supersede parts of this document):**
> 1. **The event bus exists.** §8.5's open point is resolved: EventBridge bus
>    `spatialport-events` (us-west-2) is live with a CloudWatch audit rule
>    (`/spatialport/events`), a 90-day replay archive and DLQ
>    `spatialport-events-dlq`. `nxto-api` already emits `task.created`,
>    `task.updated`, `approval.completed`. Do NOT build the JSONL fallback.
> 2. **NXTO API is pipeline-ready** (open decision 4 resolved): machine auth via
>    `x-api-key`, `POST /brain/task-proposals` with `idempotency_key` live and
>    tested (retry returns `duplicate: true`); `GET /brain/tasks?client_id=`
>    serves the wire-format view.
> 3. **GitHub plan is Free** (open decision 1 answered empirically: branch
>    protection API returned 403 on `spatialport-brain-spec`). Either upgrade to
>    Team or apply the documented fallback in §3.2.
> 4. Spec version drift fixed: `brain-spec/README.md` now says `1.1.0` and
>    documents the dual timestamp rule (open decision 8 resolved: both accepted).
> 5. `client-index.md` rows completed for all three clients (routing table no
>    longer stale — §8.1 prerequisite met).

Target state: a new team member is productive in **half a day**, with **zero
one-off authorizations**, because access derives from team membership and every
procedure is written down once and installed rather than explained.

**Destination: `spatialport-agency-brain/50-operations/internal-setup.md`.**
Not `spatialport-playbooks` — that vault is declared client-fact-free ("Client-specific
facts do not belong here") and is readable by the provider. This document names
clients and describes the provider boundary, so it belongs in the agency brain.
The *procedures* extracted from it (bootstrap, onboarding, ingestion) go to
playbooks in generic form.

> **Design note.** Much of the grammar already exists. `spatialport-brain-spec`
> defines the record schema (including `access_scope: provider`), the access
> policy (including a `Contractor` audience), the event catalog and the agent
> subscription contract. What is missing is the **enforcement layer** — and the
> spec says so itself: *"Folder location is not a permission system. Enforcement
> happens at repository permissions, retrieval filters, API scopes and signed
> asset URLs."* This document builds the first of those four and the CI around it.
>
> Three items here are **genuinely new policy and need your explicit sign-off**,
> because they loosen the current posture rather than implement it: provider write
> access on client brains (§2.3), team merge rights on evidence (§3.1), and
> auto-merge of validated evidence PRs (§8.6).

---

## 1. Principles

1. **Default deny.** Access is granted by role, never by request-and-forget.
2. **Propose, don't accept.** Humans and agents write `status: proposed`. Only
   the named `authority` accepts, and acceptance means a merged PR.
3. **The machine enforces the rules, not the discipline.** Three people plus
   several agents will not remember five rules forever. CI will.
4. **One tenant per session.** A working session touches one client. This is a
   tenancy rule first and a cost rule second — they happen to want the same thing.
5. **No secrets, anywhere, ever.** Only reference URIs: `password-manager://`,
   `aws-secretsmanager://`, `gdrive://`.
6. **The brain is the token budget.** Every accepted canon record is a compression
   of dozens of raw documents. Maintaining canon *is* the cost-control strategy.

---

## 2. Access model

### 2.1 The three roles

| Role | Who | GitHub status |
|---|---|---|
| **Authority** | Alex | Org owner |
| **Internal** | internal team member | Org member |
| **Provider** | the collaborator who is also an external service provider | **Outside collaborator** |

The third row is the important one. That person has their own GitHub account,
their own clients, and a commercial relationship that can end. They must never
be an org Member, because org members can enumerate the org's repos, teams and
people. Outside collaborators see only the repos they are explicitly added to.

### 2.2 The hard boundary

`spatialport-agency-brain` contains account overlays: margins, success-fee
schedules and capacity. At least one of those overlays carries commercial terms
explicitly marked as never to be disclosed in client deliverables. (Deliberately
not restated here — see the destination note above.)

**The provider gets no access to `spatialport-agency-brain`. Not read. Ever.**
This is not about trust; it is about not putting someone in a position where
their other engagements and your commercials sit in the same context window.

### 2.3 Permission matrix

| Repo | Authority | Internal | Provider |
|---|---|---|---|
| `spatialport-agency-brain` | admin | write | **none** |
| `spatialport-brain-spec` | admin | write (PR only) | read |
| `spatialport-playbooks` | admin | write | read |
| `spatialport-client-template` | admin | write | read |
| `client-<x>-brain` | admin | write | write — **assigned clients only** |
| `<client>-portal` (deploy repos) | admin | write | write — assigned only |

**Known gap in this row.** The access policy scopes a Contractor to "assigned
task inputs and approved output locations," and forbids "canon beyond task need."
Repo-level `write` on GitHub cannot express that: write means clone, and clone
means the whole canon. The tenant is narrowed, the scope is not. Two options —
accept the gap explicitly as a documented deviation, or move provider
contributions to a fork-and-PR model where they never hold a full clone of the
origin. Decide before granting the first provider seat; do not leave it implicit.

> **Recommendation on file (Claude, 2026-08-20): fork-and-PR.** It also solves
> the offboarding gap in §10.2b (a fork can be revoked/deleted; a clone cannot)
> and is the faithful implementation of the Contractor policy.

### 2.4 Teams

Create these GitHub teams. Onboarding then becomes *one* action per person.

```
@spatialport/authority      → admin everywhere
@spatialport/internal       → write on all repos except deploy-prod approval
@spatialport/client-marmareos
@spatialport/client-farmacia-vitalis
@spatialport/client-thi-land
```

Per-client teams are what make the provider boundary workable: you add them to
`@spatialport/client-marmareos` and they get Marmareos and nothing else. When the
engagement ends, you remove one membership.

### 2.5 Org settings — set these once

- **Require 2FA for all members and outside collaborators.** Non-negotiable.
- Base permission for members: **No permission** (grant only via teams).
- Disallow outside collaborators from being granted admin.
- Enable **secret scanning** and **push protection** on all repos.
- Restrict who can create public repos to owners only.

### 2.6 Contract note

The `ip_owner` field records an outcome; it does not create one. For work
produced by an external provider, ownership follows the contract, and the
frontmatter should reflect it (the schema allows `client`, `spatial-port`,
`videogo`, or an explicit exception — use the exception rather than guessing).
Confirm there is an IP assignment clause covering their output before their first
record is merged. I am not a lawyer and this is not legal advice — worth a look
from yours.

---

## 3. Repository topology

```
spatialport/
├── spatialport-brain-spec        the grammar. Changes are rare and gated.
├── spatialport-playbooks         reusable HOW. No client facts.
├── spatialport-client-template   the scaffold for a new client brain.
├── spatialport-agency-brain      SP's own brain + account overlays. Internal only.
├── client-marmareos-brain        \
├── client-farmacia-vitalis-brain  } one per client. Single tenant, hard boundary.
├── client-thi-land-brain         /
├── venture-suitebox-brain        internal venture (all IP Spatial Port)
└── <client>-portal               deploy repos (to be created)
```

### 3.1 CODEOWNERS

Add to every `client-*-brain` repo as `.github/CODEOWNERS`:

```
# Default: anyone on the client team may review
*                       @spatialport/client-<x>

# Authoritative surfaces: Alex only
/00-manifest/           @spatialport/authority
/10-canon/              @spatialport/authority
/30-decisions/          @spatialport/authority
/60-approvals/          @spatialport/authority
/50-tasks/              @spatialport/authority
/70-performance/        @spatialport/authority
/80-deliverables/       @spatialport/authority
/CLAUDE.md              @spatialport/authority
```

`50-tasks/` is on that list for a different reason than the others: hard rule two
says it is a generated read-only view. Leaving it team-writable is an open
invitation to hand-edit task state in Markdown, which is exactly what the rule
forbids. CI checks it too (§6).

This is the fix for the bottleneck problem. `20-evidence/` is `proposed` by
definition and non-authoritative, so the team can merge it. Canon, manifest and
decisions stay with the authority. You keep control where it matters and stop
being the queue for everything else.

### 3.2 Branch protection on `main`

- Require a pull request before merging.
- Require review from Code Owners.
- Require the `brain-validate` status check to pass.
- Block force pushes and deletions.
- **Do not** allow bypass for admins on client repos.

> **Confirmed:** the org is on Free (branch-protection API returned 403 on
> `spatialport-brain-spec` during setup). Until upgraded: CI check alone (blocks
> the merge button) + the no-push-to-main convention. Upgrade to Team is the
> recommended path.

---

## 4. Local workspace layout

Identical on all three laptops. This is what makes a session cheap and a tenancy
breach unlikely.

```
~/spatialport/
├── spec/          spatialport-brain-spec
├── playbooks/     spatialport-playbooks
├── agency/        spatialport-agency-brain     (not present on provider machines)
└── clients/
    ├── marmareos/
    ├── farmacia-vitalis/
    └── thi-land/
```

> Migration note: current clones live in `~/Projects/brains/` (flat). Adopt this
> layout when running `bootstrap.sh` (build item 3); do not maintain both.

**Rules:**

- Nothing else lives under `~/spatialport/`. The provider's other clients live
  somewhere else entirely, and never in a sibling directory.
- A Claude session opens **one** client directory. Not `~/spatialport/clients/`.
  Not the home directory. One client.
- Before writing any record: `git pull --rebase`. A stale clone produces evidence
  written against superseded canon — and it does so in perfectly good faith,
  which is what makes it dangerous.

---

## 5. Branch and PR conventions

### 5.1 Branch names

```
evidence/<client>/<yyyy-mm-dd>-<slug>
canon/<client>/<record>-<slug>
brief/<client>/<service-path>
spec/<slug>
playbook/<function>-<slug>
```

### 5.2 Lifetimes

| Branch type | Max life | Why |
|---|---|---|
| `evidence/*` | days | append-only, unique filenames, never conflicts |
| `canon/*` | **48 hours** | Markdown prose does not merge semantically |
| `spec/*` | as needed | rare, high-scrutiny |

Evidence never conflicts by design — one file per event, filename
`YYYY-MM-DD__source__topic__record-id.md`. Canon is the conflict surface. Keep
canon branches short and small; a week-old canon branch is guaranteed pain.

### 5.3 PR template

`.github/pull_request_template.md`:

```markdown
## What changed
<!-- one line -->

## Record types touched
- [ ] evidence   - [ ] knowledge (canon)   - [ ] decision
- [ ] brief      - [ ] approval            - [ ] policy

## Checks
- [ ] Every new record has complete frontmatter
- [ ] Nothing is set to `status: accepted` by me unless I am the `authority`
- [ ] No raw transcripts, raw email or raw ad operations
- [ ] No secrets — reference URIs only
- [ ] `client_id` matches this repository
- [ ] Candidate tasks were sent to NXTO, not written here

## Source
source_ref:
```

---

## 6. CI gate — `brain-validate`

**Build this first.** Before the deploy automation, before the ingestion
pipeline. It is small, it is zero-risk, and it converts the five hard rules from
intentions into mechanics. The spec already asks for it: *"A secret scanner runs
before every automated commit."*

A GitHub Action on every PR to **every brain repo — client brains *and*
`spatialport-agency-brain`** (that one holds the margins; excluding it would be
backwards). It fails if any changed `.md`:

1. Is missing required frontmatter. All fourteen: `id`, `client_id`,
   `record_type`, **`service_path`**, `status`, `owner`, `authority`, `ip_owner`,
   `access_scope`, `sensitivity`, `source_ref`, `schema_version`, `created_at`,
   `updated_at`.
2. Has a `client_id` that does not match the repository's tenant (`spatial-port`
   for the agency brain).
3. Sets `access_scope: client` without `status: accepted` — an explicit
   validation rule in `record-schema.md`.
4. Uses a `record_type`, `service_path`, `access_scope`, `sensitivity` or
   `ip_owner` outside the controlled vocabulary.
5. Matches a secret pattern (AWS keys, bearer tokens, `.env` assignments,
   long-lived signed URLs).
6. Duplicates an existing `id`.
7. Lands in `20-evidence/` without the `YYYY-MM-DD__source__topic__id` filename.
8. Modifies a file already at `status: accepted` without adding a
   `superseded_by` / `supersedes` link (see §6.1).
9. Touches `50-tasks/` at all — hard rule two, that view is generated.
   (Exception: the machine identity used by the task-sync automation.)
10. Trips the raw-content heuristic in `20-evidence/`: dialogue markers,
    speaker-turn density, `From:`/`To:`/`Subject:` blocks. A blunt proxy for hard
    rule five, but it catches the obvious case of someone pasting a transcript.

**Acceptance is enforced at merge, not in CI.** Hard rule three says *"Canon
acceptance = PR merged by Alex only,"* and CI cannot see who clicks merge. That
control is CODEOWNERS plus branch protection (§3.1, §3.2). CI's job is shape;
branch protection's job is authority. Do not conflate them.

Same workflow file in every brain repo. When it changes, it changes in all of
them — use a reusable workflow in a `.github` repo rather than copy-paste.

### 6.1 Supersession

`record-schema.md`: *"Accepted knowledge can be superseded, never silently
rewritten without history."* Convention: an accepted record is never edited in
place. A new record is written with a new `id` and `supersedes: <old-id>`; the
old record gets `superseded_by: <new-id>` and keeps its content. Check 8 above
enforces it.

---

## 7. Token-efficiency SOP

Rules, in order of how much they save.

**1. Never read a repo through a browser.** Clone it.
**2. Search, don't read.** `Grep`/`Glob` across a clone, then read only matches.
**3. One client per session.** Cheaper *and* it enforces tenancy.
**4. Let `CLAUDE.md` do the briefing.** Each brain repo carries its own
governance. (Caveat: convenience, not control — see §8.7.)
**5. Delegate wide searches to a subagent.** A subagent that reads thirty files
returns a conclusion, not thirty files.
**6. Maintain canon, and the reading gets cheap.** Canon debt is token debt.
**7. Batch the small questions.** Five questions in one session beats five
sessions.

---

## 8. Ingestion pipeline

Two entry points, one destination shape.

```
  Google Meet transcript                Document uploaded in Claude
           │                                        │
           ▼                                        ▼
  ┌─────────────────────────────────────────────────────────┐
  │ 1. ROUTE      → client_id, or triage. Never guess.       │
  │ 2. REDACT     → summary + source_ref. Raw stays out.     │
  │ 3. EMIT       → evidence.md (proposed)                   │
  │                 candidate canon / decisions              │
  │                 candidate tasks → NXTO API               │
  │ 4. OPEN PR    → machine identity, branch evidence/…      │
  └─────────────────────────────────────────────────────────┘
```

### 8.1 Routing — the dangerous step

- Routing table: `spatialport-agency-brain/20-portfolio/client-index.md`
  (now complete for all three clients + the Suitebox venture).
- Signal order: calendar event → attendee domains → title convention → explicit tag.
- **Confidence below threshold, or two clients in one meeting → triage queue.**
  The pipeline never guesses a tenant.

### 8.2 Redaction — not optional

Hard rule five: raw transcripts never enter the repo. What lands is a redacted
note carrying `source_ref: gdrive://<file-id>`.

### 8.3 Output shape

Follow `20-evidence/README.md` literally: source, speakers/actors, redaction
result, factual summary, direct implications, candidate tasks, candidate
decisions, candidate canon — never merged into one object.

### 8.4 Two destinations

- Evidence + candidate canon → **GitHub PR**, `status: proposed`.
- Candidate tasks → **NXTO**, `POST {NXTO_API}/brain/task-proposals`, with
  `idempotency_key`, `title`, `client_id`. Live and tested. Never written into
  the repo (hard rule two).

### 8.5 Idempotency and identity

- Derive record `id` and filename deterministically from the meeting/file ID.
- Commits from a **GitHub App or machine account**, never a personal PAT.
- Emit the controlled events to the **existing** EventBridge bus
  `spatialport-events` (us-west-2): the ingestion path emits `evidence.created`,
  `canon.proposed`, `task.created` (the last already emitted by nxto-api on
  proposal); `canon.accepted` fires on merge; `approval.completed` and
  `output.ready` from the delivery lane; `client.offboarded` from §10.

### 8.6 Review-debt control

- **Batch:** one evidence PR per client per day, not one per event.
- **Auto-merge** evidence PRs that pass `brain-validate` (pending sign-off).
- **Expire:** a candidate-canon PR untouched for 14 days closes with a comment.

### 8.7 The missing lane: delivery to the client

The access model covers three of the four audiences and never opens the client
lane. `access_scope: client` currently governs a path that does not exist.
Also unbuilt: retrieval filters, API scopes, signed asset URLs — three of the
four enforcement points the spec names. The agent subscription contract
(`read_scopes`, `write_scopes`, `forbidden_scopes`, `human_authority`) is the
design that governs them. Not solved here; see §11 items 9–10.

---

## 9. Onboarding — the half day

**Prerequisite (Alex, ~5 minutes):** add the person to the right GitHub teams and
share the password-manager collections their role needs.

| Block | Duration | What |
|---|---|---|
| 1 | 30 min | Accept GitHub invite, enable 2FA, install `gh` CLI and authenticate |
| 2 | 30 min | Run `bootstrap.sh` → clones the repos they have access to into `~/spatialport/` |
| 3 | 45 min | Read `spatialport-brain-spec`: `record-schema.md`, `access-policy.md`, `approval-policy.md` |
| 4 | 30 min | Install the Spatial Port plugin in Cowork → skills, connectors and conventions in one install |
| 5 | 45 min | Read one real client brain end to end (Marmareos is the most complete) |
| 6 | 60 min | Supervised first PR: one evidence record from a real document, watch `brain-validate` run |

**Build list for this to be true:** `bootstrap.sh` in playbooks · Spatial Port
Cowork plugin (skills: `brain-ingest`, `brain-query`, `client-onboard`) ·
`ONBOARDING.md` in playbooks.

---

## 10. Offboarding

1. Remove from all teams and outside-collaborator lists.
2. Revoke password-manager collection shares.
2b. **Local clones survive revocation.** Handle contractually (deletion
   attestation) — technically you cannot. Strongest argument for fork-and-PR
   (§2.3 recommendation).
3. Rotate anything they held that is not OIDC-based.
4. Emit `client.offboarded` where a client relationship also ends.
5. Their commits stay. History is not rewritten.

---

## 11. Build order

| # | What | Why here |
|---|---|---|
| 1 | Org settings, teams, CODEOWNERS, branch protection (or Free fallback) | Everything else assumes it |
| 2 | `brain-validate` CI | Turns rules into mechanics before volume arrives |
| 3 | `bootstrap.sh` + `ONBOARDING.md` | Makes the half day real |
| 4 | Spatial Port Cowork plugin + skills | Homogeneity across three laptops |
| 5 | ~~Fix client-index / spec versions~~ **done 2026-08-20** | — |
| 6 | Ingestion: document upload path first | Manual trigger, easier to debug |
| 7 | Ingestion: Google Meet path | Automatic trigger once the shape is proven |
| 8 | Portal repos + OIDC + deploy workflow | Valuable, but least foundational |
| 9 | Agent subscription contracts | Scoped retrieval instead of blanket repo access |
| 10 | Client delivery lane: retrieval filters, API scopes, signed URLs | Opens `access_scope: client` |

---

## 12. Decisions (signed off by Alex, 2026-08-20)

1. **GitHub plan — DEVIATION, signed.** No upgrade for now; colleagues work
   **on Alex's own GitHub account**. Recorded consequences: CODEOWNERS and
   per-person audit trail are inert (every commit reads as Alex); the
   "acceptance = merged by Alex" control degrades to convention + CI shape
   checks; offboarding a colleague means rotating Alex's credentials.
   **Revisit trigger:** the first external provider seat, or the first
   colleague working on client brains weekly — whichever comes first. At that
   point: separate accounts + Team plan.
2. ~~Spec version drift~~ **Fixed: 1.1.0 everywhere.**
3. **Provider scope.** Open — list the provider's clients when the first seat
   is granted (and see 9: it will be fork-based).
4. ~~NXTO API~~ **Answered: live** (machine identity via `x-api-key`,
   idempotent proposals working).
5. **Meet transcription source.** Open — Gemini notes into Drive vs third-party.
6. **Auto-merge on evidence — SIGNED yes**, with daily batching per client and
   14-day canon expiry. Implemented in `brain-validate.yml` + `canon-expire.yml`.
7. **Language.** English. Confirmed.
8. ~~Timestamp format~~ **Fixed: both accepted.**
9. **Provider scope model — SIGNED: fork-and-PR.** Providers never hold write
   on origin repos; they fork, PR, and the fork access is revocable. Also
   resolves §10.2b.

### Implementation status (2026-08-20)

- `brain-validate` CI: built (`playbooks/tools/brain-validate.mjs` + reusable
  workflows), installer `playbooks/tools/install-brain-ci.sh` distributes it to
  all five brain repos. Build item 2: **done pending install run**.
- Evidence auto-merge + canon expiry: built into the same workflows (item §8.6).
- `bootstrap.sh` + `ONBOARDING.md`: built in playbooks. Build item 3: **done**.
- Remaining build items: 4 (Cowork plugin), 6-7 (ingestion), 8 (portals), 9-10
  (subscriptions + client lane).
