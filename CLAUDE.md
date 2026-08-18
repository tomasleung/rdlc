# Foster Analysis — Project Instructions

Animal Flow — Foster Pathway Analytics, Phase 1 (BC SPCA). RDLC OS project — see `README.md` for the full agent-chain narrative; this file is the short, operational version Claude Code loads automatically every session.

## Project Structure

```
foster-analysis/
├── README.md                  # Full agent-chain narrative — read this for the "how it all fits" story
├── CLAUDE.md                  # This file
├── .claude/skills/             # The 3 RDLC OS agents, embedded in this project
│   ├── rdlc-coach-semantic-model/
│   ├── rdlc-mock-data-generator/
│   └── rdlc-tmdl-build-agent/
├── 01-inputs/                  # BRD, Discovery Framework, and the rough draft table definitions (v1.0) — everything fed INTO rdlc-coach-semantic-model
├── 02-table-definitions/       # Everything the agent PRODUCED — v1.1 (first reviewed/confirmed) through current. Never delete old versions.
├── 03-mock-data/                # CSV fixtures from rdlc-mock-data-generator
└── 04-powerbi-project/          # PBIP/TMDL output from rdlc-tmdl-build-agent
```

## Pipeline Order — Non-Negotiable

`01-inputs` → `rdlc-coach-semantic-model` confirms → `02-table-definitions` (latest) → `rdlc-mock-data-generator` → `03-mock-data` → `rdlc-tmdl-build-agent` → `04-powerbi-project`

Never skip a stage. Each skill has its own non-negotiable gate (see its `SKILL.md`) — do not attempt a later stage's work from an earlier one. If a required upstream artifact is missing, say so and stop; do not author it yourself from the wrong agent.

## Which Skill Applies

- Reviewing, questioning, or refining a draft Table Definitions document → `.claude/skills/rdlc-coach-semantic-model/SKILL.md`
- Generating mock/sample data for a confirmed model → `.claude/skills/rdlc-mock-data-generator/SKILL.md`
- Building the actual Power BI semantic model (PBIP/TMDL) → `.claude/skills/rdlc-tmdl-build-agent/SKILL.md`

Read the relevant skill's `SKILL.md` before acting — this file intentionally does not duplicate their contents.

## Current State

- **Current confirmed Table Definitions**: `02-table-definitions/TABLE_DEFINITIONS_FOSTER_v1_3.docx`
- **Mock data**: `03-mock-data/` — 731-day contiguous range (2024-01-01 to 2025-12-31), 32 centres / 5 regions, 40 animals, 34 intake types / 8 groups, 600 fact rows, seeded (seed=42) for reproducibility.
- **PBIP project**: `04-powerbi-project/Foster Analysis.SemanticModel/`

## Before Opening in Power BI Desktop

Update the `DataFolder` parameter in `04-powerbi-project/Foster Analysis.SemanticModel/definition/model.tmdl` to this machine's actual absolute path to `03-mock-data/`.

## Working Conventions

- Table Definitions documents are never overwritten — a new version is created, with a "Change from v(n-1)" note. `01-inputs/` holds your original rough draft (v1.0); `02-table-definitions/` holds every reviewed/confirmed version `rdlc-coach-semantic-model` has produced since (v1.1+). Clean rule: `01` = fed into the agent, `02` = produced by the agent.
- Mock data (`03-mock-data/`) is Prototype-mode only — never treat it as, or mix it with, real ShelterBuddy/production data.
- `rdlc-tmdl-build-agent` authors files directly — MCP (`powerbi-modeling-mcp`) is only used for post-build validation or live Fabric ID discovery, never as the default authoring loop.

## Known Gaps (flag to the human — do not silently resolve)

- Species Group mapping is incomplete (Cat/Dog/Small Animal confirmed; "Other" = not yet mapped)
- Municipality-level geography is deferred — no source data yet
- EAB/LSAI Program derivation from Intake Type/Group is unconfirmed — Phase 2 concern
- BRD §7.1 (`01-inputs/`) still lists Foster Type/Outcome/Placement Dates as Phase 1 signals — known, accepted inconsistency; the Table Definitions document is the source of truth for Phase 1 scope, not the BRD
