# rdlc-mock-data-generator

Part of **RDLC OS** — a decision-driven BI development methodology.

**Role:** Data Prototyping Agent
**Position in pipeline:** Sits downstream of `rdlc-coach-semantic-model` (which produces the confirmed Table Definitions document) and upstream of the future Semantic Model Build Agent. Generated CSVs serve as a Prototype-mode (M/Import) partition source, later swapped to a Fabric warehouse (Direct Lake / Entity) source once real data exists — same table/column/measure authoring either way, only the partition source property changes.

**What this skill does:** Takes a *confirmed* Table Definitions document and generates one structurally-valid, referentially-intact CSV per table. Asks the human for a volume (row count) per dimension table — recommending a sensible default when asked — then derives the fact table's size and distribution from those dimension volumes and the confirmed grain, rather than asking for an arbitrary fact row count directly.

**What this skill does NOT do:** Review or author the Table Definitions document itself, build or touch a semantic model / TMDL / Power BI project, or use/approximate any real source-system data.

## How to use

Point the assistant at this folder alongside a confirmed Table Definitions document (e.g., `TABLE_DEFINITIONS_FOSTER_v1_2.docx`). It will verify the document is confirmed, then run a short volume-elicitation conversation before generating files.

### In a paste-only environment
Paste `SKILL.md`, then paste or describe `references/output-format-spec.json` before asking for final CSV output — the volume-elicitation conversation itself can proceed without it.

## Folder structure

```
rdlc-mock-data-generator/
├── SKILL.md                          # Root control file — role, gates, workflow, volume-elicitation protocol
├── README.md                         # This file
└── references/
    ├── mock-data-standards.md        # Referential integrity, realistic distributions, volume calibration (soft guidance)
    ├── output-format-spec.json       # Hard, deterministic CSV format constraints
    └── gold-example/                 # Real, worked example: BC SPCA Foster Analysis mock fixtures
        ├── Dim_Date.csv
        ├── Dim_Centre.csv
        ├── Dim_Animal.csv
        ├── Dim_Intake.csv
        └── Fact_AnimalIntake.csv
```

## Required input

A confirmed Table Definitions document (must have passed `rdlc-coach-semantic-model`'s handoff gate). Rough/unconfirmed drafts are refused — this skill mocks a confirmed schema, it does not help design one.
