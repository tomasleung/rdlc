# Foster Analysis — RDLC OS Project

Animal Flow — Foster Pathway Analytics, Phase 1 (BC SPCA). Built using **RDLC OS**, a decision-driven, human-gated BI development methodology: an LLM assists at each pipeline stage, but a human orchestrator retains decision authority throughout.

This document explains how the three RDLC OS agents in this project chain together. It's the narrative companion to `CLAUDE.md` (short, operational, loaded automatically by Claude Code every session) — read this for the full story; `CLAUDE.md` for quick operational facts.

For the status of the RDLC OS agent ecosystem as a whole (which agents exist, what's still being built, cross-project design decisions), see `RDLC_OS_Agent_Status_Tracker.md` at the project root — note it's a *temporary* home for that file, not project-specific content.

*Only three agents exist today. This document will be updated as more agents (e.g., the upstream Senior Data Analyst agent, or a future report-authoring agent) are added to the chain.*

---

## The Chain

```
01-inputs/                              02-table-definitions/
┌─────────────────────────┐             ┌──────────────────────────┐
│ BRD                     │             │ TABLE_DEFINITIONS         │
│ Discovery Framework     │             │ _FOSTER_v1_1.docx          │
│ TABLE_DEFINITIONS        │──────┐      │ ...                       │
│ _FOSTER_v1_0.pdf (rough) │      │      │ _v1_3.docx (CONFIRMED)    │
└─────────────────────────┘      │      └──────────────────────────┘
                                  │                    │
                                  ▼                    │
                    ┌──────────────────────────┐       │
                    │ rdlc-coach-semantic-model│       │
                    │ (Data Architect + Data   │       │
                    │  Modeler — QA/review)    │───────┘
                    │                          │
                    │ Grills the human,        │
                    │ one round at a time,     │
                    │ against Kimball + MS     │
                    │ Fabric/Power BI          │
                    │ standards. Never authors │
                    │ from scratch, never      │
                    │ builds.                  │
                    └──────────────────────────┘
                                  │
                     (confirmed Table Definitions
                      — handoff gate passed)
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │ rdlc-mock-data-generator │
                    │ (Data Prototyping Agent) │
                    │                          │
                    │ Grills the human for     │
                    │ per-table row-count      │
                    │ volumes, then generates  │
                    │ referentially-valid      │
                    │ CSV fixtures.            │
                    └──────────────────────────┘
                                  │
                                  ▼
                        03-mock-data/
                        Dim_Date.csv, Dim_Centre.csv,
                        Dim_Animal.csv, Dim_Intake.csv,
                        Fact_AnimalIntake.csv
                                  │
                                  ▼
                    ┌──────────────────────────┐
                    │ rdlc-tmdl-build-agent    │
                    │ (Implementation Agent —  │
                    │  Step 13, runs in        │
                    │  Claude Code)            │
                    │                          │
                    │ Authors PBIP/TMDL files  │
                    │ directly on disk (no MCP │
                    │ as primary loop). Static │
                    │ self-validates before    │
                    │ declaring a build done.  │
                    └──────────────────────────┘
                                  │
                                  ▼
                        04-powerbi-project/
                        Foster Analysis.SemanticModel/
```

---

## Agent 1 — `rdlc-coach-semantic-model`

**Role:** Data Architect + Data Modeler. QA and questioning only — it never authors a first-draft model, never builds anything.

**Input:** A BRD, a Discovery Framework, and a *rough* draft Table Definitions document (from `01-inputs/`). The draft is expected to be imperfect — catching grain errors, wrong data types, and unjustified columns is this agent's entire purpose.

**How it works:** Follows the mattpocock grilling protocol — batches independent questions into rounds (`❓ Qn` / `➡️` recommended answer), asks the human to confirm or explicitly defer to the recommendation, and never treats a deferral with the same confidence as a direct answer. It applies Kimball dimensional modeling principles and Microsoft Fabric/Power BI standards (including corrections sourced from Microsoft's own `semantic-model-authoring` skill and Tabular Editor's Best Practice Analyzer rules, with documented conflict resolution where the two disagree).

**Non-negotiable gate:** `NO CONFIRMED GRAIN → NO FACT TABLE`.

**Output:** An updated, versioned Table Definitions document in `02-table-definitions/`, with every material change recorded in a "Change from v(n-1)" note and a revision history table. Only agent-reviewed/confirmed versions live here (v1.1+) — the original rough draft (v1.0) stays in `01-inputs/` alongside the BRD and Discovery Framework, since it's what was fed *into* this agent, not what it produced. Never delete old versions; the "what changed and why" trail matters.

**Handoff gate to the next agent:** every fact table has a human-confirmed grain statement; every column is justified or explicitly marked as a deliberate scope cut; zero unresolved "Assumed — pending Data Owner" items (or an explicit human override).

---

## Agent 2 — `rdlc-mock-data-generator`

**Role:** Data Prototyping Agent. Turns a *confirmed* Table Definitions document into realistic, structurally-valid mock data — enabling model-first prototyping before any real Fabric warehouse data exists.

**Input:** The confirmed Table Definitions document from `02-table-definitions/`. Refuses to run against a draft.

**How it works:** Asks the human for a row-count volume per dimension table (with a recommended default calibrated to stated purpose — CI/CD fixture vs. stakeholder demo). The fact table's size and distribution are *derived*, not asked directly — from the confirmed grain and the dimension volumes, applying realistic (non-uniform) distributions where a real-world pattern is known (e.g., seasonal skew, categorical skew), not naive random generation.

**Non-negotiable rule:** referential integrity is mandatory — every fact-table foreign key must resolve to a real row in its dimension CSV. Validated programmatically before presenting output, every time.

**Output:** One CSV per table in `03-mock-data/`, headers using Technical Names (the ETL/source-layer naming, not the Power BI-facing Business Names) — since these files are read directly by the Power BI M/Import partition query in Prototype mode.

---

## Agent 3 — `rdlc-tmdl-build-agent`

**Role:** Implementation Agent (RDLC OS Step 13). The only agent in the chain with live tool/MCP access — and even here, MCP is a narrow secondary mode, not the default.

**Input:** The confirmed Table Definitions document (`02-table-definitions/`) plus the local path to `03-mock-data/`.

**How it works — two modes:**
- **Authoring Mode (default):** hand-authors PBIP/TMDL files directly on disk. No MCP connection opened. Applies exact TMDL syntax rules, Microsoft's modeling/AI-readiness standards, and PBIP folder conventions.
- **Verify/Setup Mode (secondary, two triggers only):** (a) validating a completed build via a narrow, live MCP session, or (b) discovering live Fabric workspace/Lakehouse IDs when migrating from Prototype (CSV) to Production (Direct Lake) mode.

**Non-negotiable rule:** runs a static, no-live-engine-required self-validation checklist before ever declaring a build complete — checking things like measure/column name collisions, missing descriptions, forbidden data types, and the documented MS-vs-Tabular-Editor conflict resolution (no `isKey` on dimension primary keys).

**Output:** `04-powerbi-project/Foster Analysis.SemanticModel/` — a complete PBIP/TMDL project, ready to open in Power BI Desktop once the `DataFolder` parameter in `model.tmdl` is pointed at this machine's local `03-mock-data/` path.

**Explicitly out of scope:** redesigning any grain/key/SCD decision already confirmed by Agent 1; configuring AI instructions, synonyms, or the AI Data Schema (Power BI's "Prep data for AI" UI only — this agent may draft suggested text but cannot apply it); authoring report/visual pages.

**Real-world validated (2026-08-18):** this agent was run for real via Claude Code — not just designed on paper. Two TMDL syntax bugs were found via actual Power BI Desktop errors and permanently fixed in the skill's reference material (see `.claude/skills/rdlc-tmdl-build-agent/usage/CHANGELOG.md`). After fixing, the model opened cleanly and all three core KPIs matched their predicted values exactly (see `.claude/skills/rdlc-tmdl-build-agent/usage/FOSTER-ANALYSIS-VALIDATION.md`). For the exact operating procedure — how to start a Claude Code session, invoke this skill correctly, and troubleshoot — see `.claude/skills/rdlc-tmdl-build-agent/usage/SOP.md`.

---

## Known Gaps (carried forward from the Table Definitions document)

- Species Group mapping incomplete — only Cat, Dog, and Small Animal confirmed by the Data Owner; "Other" is a temporary placeholder, not a final category.
- Municipality-level geography deferred — no source data yet.
- EAB/LSAI Program derivation from Intake Type/Group is unconfirmed — a Phase 2 concern.
- BRD §7.1 (source document, in `01-inputs/`) still lists Foster Type/Outcome/Placement Dates as Phase 1 signals. The Table Definitions document is the source of truth for Phase 1 scope, not the BRD — this is a known, accepted inconsistency, not an oversight.

## Skill Usage Documentation

`rdlc-tmdl-build-agent` is the first skill in this project to run via Claude Code against real local files, rather than conversationally. Its `usage/` subfolder holds the human-facing operating documentation (distinct from `references/`, which is agent-facing):

- `usage/SOP.md` — how to correctly start a Claude Code session, invoke this skill, and troubleshoot
- `usage/MS-FABRIC-MCP-SETUP.md` — one-time setup for Microsoft's real `semantic-model-authoring` skill + `powerbi-modeling-mcp` (Verify Mode), with real troubleshooting encountered during initial setup
- `usage/CHANGELOG.md` — real bugs found via actual Power BI Desktop testing, root cause, and permanent fix
- `usage/TDD-VERIFICATION.md` — the predict-then-confirm verification methodology
- `usage/FOSTER-ANALYSIS-VALIDATION.md` — the filled-in result of that methodology for this project
