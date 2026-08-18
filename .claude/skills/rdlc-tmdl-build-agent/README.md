# rdlc-tmdl-build-agent

Part of **RDLC OS** — a decision-driven BI development methodology.

**Role:** Implementation Agent (Step 13)
**Position in pipeline:** Downstream of `rdlc-coach-semantic-model` (confirmed Table Definitions document) and `rdlc-mock-data-generator` (local CSV fixtures for prototype builds). This is the only RDLC OS agent where live tool/MCP access is warranted — and even here, MCP is a narrow secondary mode, not the primary authoring loop.

**Runs in:** Claude Code, against the local file system and (optionally, for validation only) a live Power BI Desktop / Fabric connection via `powerbi-modeling-mcp`. Not designed to run in the chat interface.

**What this skill does:** Translates a confirmed Table Definitions document into an actual PBIP/TMDL Power BI semantic model — hand-authoring files directly on disk, then running a static, no-live-engine-required self-validation pass before declaring the build complete.

**What this skill does NOT do:** Redesign grain/keys/SCD/column decisions (already gated by `rdlc-coach-semantic-model`), configure AI instructions/synonyms/AI Data Schema/Verified Answers (Power BI "Prep data for AI" UI-only — this agent may draft suggested text but cannot apply it), author reports/visuals, or use MCP as its default authoring loop.

## Two modes, one agent

| | Authoring Mode (default) | Verify/Setup Mode (secondary) |
|---|---|---|
| Tools | File read/write only | `powerbi-modeling-mcp`, narrow |
| When | Every build starts here | (a) validating a completed build, (b) discovering live Fabric IDs when migrating prototype → production |

## How to use

Point Claude Code at this folder alongside a confirmed Table Definitions document and a local CSV path (or Fabric source reference). It will verify the input contract, parse the document, author the PBIP project, and self-validate before reporting back.

## Folder structure

```
rdlc-tmdl-build-agent/
├── SKILL.md                                      # Role, gates, mode selection, workflow
├── README.md                                     # This file
└── references/
    ├── tmdl-syntax-guide.md                       # Exact TMDL object syntax
    ├── modeling-and-ai-readiness-standards.md     # MS + Tabular Editor synthesis, MS-wins conflict resolution
    ├── pbip-structure.md                          # Physical folder/file scaffold
    ├── static-validation-checklist.md             # No-live-engine self-check, run before every "done"
    └── gold-example/                              # Real, worked PBIP/TMDL project (BC SPCA Foster Analysis)
```

## Required input

1. A confirmed Table Definitions document (passed `rdlc-coach-semantic-model`'s handoff gate)
2. A local data source path (CSV for prototype, Fabric reference for production)
3. A target project folder path

## Updating this skill's references

Each reference file with external-source content has a "Source Provenance" table with the originating URL and a "last verified" date. To refresh against current Microsoft/Tabular Editor guidance, ask for the sources to be re-fetched and diffed against the frozen content before any edit is made.
