---
id: sp-canon-company
client_id: spatial-port
record_type: knowledge
service_path: company
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
# Company Profile

## Identity

- **Legal entity / HQ:** Spatial Port Inc., 1648 Union Street, Suite 201, San Francisco, CA 94123, USA (Preventivo-Onniversum-Elvetica-Locker.md, quote header, July 2026).
- **Founder & CEO:** Alex Bellesia — alex@spatial-port.com · spatial-port.com (Offerta-Collaborazione-Junior-AI-Operations-Assistant.md, signature block).
- **Positioning:** a **design × technology studio**. In the company's own words: «trasformiamo spazi, prodotti e brand in esperienze digitali raffinate — siti web cinematografici, identità di marca, 3D e digital twin, piattaforme digitali e soluzioni di intelligenza artificiale» (Offerta-Lavoro-Junior-AI-Operations-Assistant.md, "Chi siamo").
- **Markets:** Italy, Switzerland, USA. Typical clients: wineries, restaurants, design studios, retail, luxury real estate, pharmacies, and SMEs that want a step change in quality (Offerta-Lavoro, "Chi siamo").

## What we do

1. **Cinematic websites & landing pages** — e.g. spatial-port.com v2 (EN+IT) with its own serverless lead engine (projects.ts, sito-spatial-port).
2. **Brand identity** — logo, brandbook, positioning, mascotte (projects.ts: Marmareos, Thi Land, Farmacia Vitalis).
3. **3D, digital twin, AI film & rendering** — sketch-to-photoreal, walkthroughs, product films, AI reels (projects.ts: IDB Villa Paradiso, Arc Living, Area 23, Osaga, Bespoke Light).
4. **Digital platforms & custom software** — internal OS (NXTO), client systems such as the Thi Land points-and-access system (projects.ts).
5. **AI automation & growth** — AI quoting tools, CRM intelligence, paid media, social (projects.ts: Marmareos phases 3–4).

**Pricing signal:** €100/h on small jobs, VAT excluded; small maintenance retainers from €25/month (Preventivo-Onniversum-Elvetica-Locker.md, July 2026 — one-off rebrand quoted at €560 net after 20% discount, first year €860).

## Delivered portfolio 2024–2026

Source: NXTO `src/data/projects.ts` (real project data, June 2026 state).

| Project | Client | Location | Scope | Status |
|---|---|---|---|---|
| Marmareos — Growth & AI | Marmareos | Verona, IT | Brand + sito (done), film Monte-Carlo (done), growth marketing, AI tools (preventivi, CRM) | active |
| Thi Land — Brand & Launch | Thi Land | Lumino, CH | Brand + mascotte + brandbook (done), sito + landing biglietti (done), sistema punti & accessi, lancio + social | active |
| Farmacia Vitalis — Brand | Farmacia Vitalis | Lumino, CH | Brand identity + brandbook | completed |
| Villa Paradiso — AI Film | IDB | Bahamas | Teaser + 12 sketch→render | completed |
| Arc Living — AI Rendering | Arc Living | Sydney, AU | Sketch→photoreal + motion | completed |
| Area 23 — 3D Viz | Area 23 | Sant'Antonino, CH | Render + walkthrough (2024) | completed |
| Osaga — New Kicks | Osaga | — | AI reel | completed |
| Bespoke Light — Film | Bespoke Light | — | Product film | completed |
| Showroom Montecarlo | Showroom MC | Monte-Carlo | Brand & digital | completed |
| Suitebox — Venture | Suitebox | — | Venture building (deck + sviluppo) | paused |
| NXTO × Spatialport OS | Spatial Port | Lugano, CH | Internal digital platform | active (internal) |
| spatial-port.com | Spatial Port | Lugano, CH | Site v2 + lead engine (Lambda+SES) | active (internal) |

Recent commercial activity beyond the NXTO list: Onniversum CH → **Elvetica Locker** site rebrand + domain migration quote, July 2026 (Preventivo-Onniversum-Elvetica-Locker.md); real invoice archive covers Marmareos (0343–0348), Showroom Montecarlo, MB&DP SAGL renderings, Onniversum CH, Bespoke Lighting (`SITO SPATIALPORT/INVOICES SPATIAL PORT - CLIENTS/`).

## Sales & demo system

- Reusable demo/sales-asset system on subdomains `*.spatial-port.io`, deployed on AWS (S3 + CloudFront), one idempotent setup/deploy script pair per demo (`SITO SPATIALPORT/client-demos/*/setup-domain.sh`, `deploy.sh`).
- Live demo assets: **Great Bear Vineyards** (v1 + v2 multi-page), **Nizza Milano** (restaurant), **Ron Mann Design** (design studio), **Scott sales kit** (proposal + playbook + Great Bear hook) (`SITO SPATIALPORT/client-demos/` folder contents).

## Accelerator & financial posture

- Spatial Port participates in the **Startupbootcamp** accelerator program with **CDP Venture Capital**; program contact Ludovica Dauri. Q1 2026 report filed as `Report Q1 2026 Spatial Port Inc.xlsx` (Downloads). **FY2025 closed at break-even, no loss** (conversation record 2026-08).

## Operating model — AI-native

- «Siamo un'azienda AI-native. Usiamo Claude, ChatGPT e automazioni ogni singolo giorno per fare in poche ore ciò che altri fanno in settimane» (Offerta-Lavoro, "Chi siamo").
- **NXTO OS** — the internal operating dashboard, live at **os.spatial-port.io** with JWT+TOTP auth; backend on AWS (DynamoDB + Lambda, us-west-2). Pages: /today (cockpit), /command (directives), /projects, /pipeline, /agents, /email, /finance, /clients, /settings (NXTO CLAUDE.md, state 2026-06-05).
- **Agent organization:** Alex → **Nexus** (COO, delegates, never executes) → 8 operating leads — Atlas (PM), Ledger (Finance), Hunter (Sales), Quill (Content), Prism (Design/3D/AI Visual), Forge (Dev), Scout (Strategy), Echo (Client Care) — → **Sentinel** (QA, max 2 iterations, never rewrites). 10 leads + ~45 specialists in the v3 catalog; graduated autonomy L1–L3 and monthly action budgets per agent (NXTO `src/data/agents.ts`, SPRINTS.md Sprint 8–9).
- **Brains system:** governed knowledge repos — this agency brain plus per-client brains; records are proposed by AI sessions and accepted only by Alex (brain CLAUDE.md).
- Task state lives in NXTO, not in Markdown; every directive transition is audit-logged (nxto-activity) (NXTO CLAUDE.md).

## Hiring state (August 2026)

- **Junior AI Operations Assistant** — open role: Partita IVA, 20 h/week remote, Italian working language, path €800/month (months 1–3) → €1,000 (month 4) → €1,200 (month 7); «il futuro braccio destro operativo di Spatial Port» (Offerta-Lavoro-Junior-AI-Operations-Assistant.md; offer template Offerta-Collaborazione-Junior-AI-Operations-Assistant.md).
- **Chief of Staff & Operations** — peer proposal under evaluation: €1,500/month, 15–20 h/week, renewable 2 months (conversation record 2026-08).

## Known data caveats

- NXTO `src/data/finance.ts` is explicitly labelled mock («cifre plausibili ma FITTIZIE»); real invoices live in `SITO SPATIALPORT/INVOICES SPATIAL PORT - CLIENTS/`. Reconcile before using figures externally (finance.ts header; SPRINTS.md T-142).
