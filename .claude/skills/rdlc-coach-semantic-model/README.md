# rdlc-coach-semantic-model

Part of **RDLC OS** — a decision-driven BI development methodology.

**Role pairing:** Data Architect + Data Modeler
**Position in pipeline:** Sits between the upstream PM + Senior Data Analyst agent (which produces the BRD, Data Discovery Framework, and an initial draft Table Definitions document) and the downstream Step 13 Implementation Agent (Fabric/MCP build).

**What this skill does:** QA and coaching only. It grills the human orchestrator, one round of questions at a time, applying Kimball dimensional modeling and Microsoft Fabric/Power BI semantic model best practices, until every open design decision in a draft Table Definitions document is either explicitly confirmed or explicitly deferred. It then produces an updated, deterministically-formatted Word document recording the confirmed model.

**What this skill does NOT do:** Author a first-draft semantic model from scratch, build/deploy anything in Fabric, or send/track any outreach to a Data Owner.

## How to use

### In a repo/file-aware environment (Claude Code, Cursor, Claude Projects, etc.)
Point the assistant at this folder. `SKILL.md` will instruct it to load `references/` on demand.

### In a paste-only environment (e.g., M365 Copilot Chat with no file access)
Paste the contents of `SKILL.md` into a new chat, then follow its degraded-mode instructions — it will ask you to also paste `references/output-format-spec.json` (and describe or paste key parts of the gold example) before it produces a final formatted document. The grilling/QA conversation itself can begin even without the reference files.

## Folder structure

```
rdlc-coach-semantic-model/
├── SKILL.md                              # Root control file — role, gates, workflow, grilling protocol
├── README.md                             # This file
└── references/
    ├── semantic-modeling-standards.md    # Kimball + MS Fabric/Power BI reasoning anchors (soft guidance)
    ├── output-format-spec.json           # Hard, deterministic output document constraints
    └── gold-example/
        └── TABLE_DEFINITIONS_FOSTER_v1_2.docx   # Real, worked example of correct final output (5-column Technical/Business name schema)
```

## Required input

1. BRD
2. Data Discovery Framework
3. Draft Table Definitions (rough is fine — grain errors, wrong types, and unjustified columns are expected and are exactly what this skill is for)

If any of these three are missing, the skill will halt and ask for them rather than authoring them itself.
