# PBIP Structure Guide

Physical folder/file scaffold for a Power BI Project (PBIP), as used by RDLC OS Phase 1 builds.

## Source Provenance
| Source | URL | Last Verified |
|---|---|---|
| MS skills-for-fabric — semantic-model-authoring (pbip.md) | https://github.com/microsoft/skills-for-fabric/tree/main/plugins/powerbi-authoring/skills/semantic-model-authoring | 2026-08-17 |

If asked to update this file, re-fetch the URL above, diff against this content, and report changes before editing.

---

## Semantic-Model-Only Scaffold (RDLC OS default — no report authored by this agent)

RDLC OS Phase 1 builds produce the **semantic model only**; report/visual authoring is a separate, later concern not owned by this agent.

```text
[ProjectName]/
└── [ModelName].SemanticModel/
    ├── definition.pbism
    └── definition/
        ├── database.tmdl
        ├── model.tmdl
        ├── relationships.tmdl
        └── tables/
            ├── Date.tmdl
            ├── Centre.tmdl
            ├── Animal.tmdl
            ├── 'Intake Type'.tmdl
            └── 'Animal Intakes'.tmdl
```

## definition.pbism

Create exactly as shown — no modification needed:

```json
{
    "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/semanticModel/definitionProperties/1.0.0/schema.json",
    "version": "4.2",
    "settings": {
        "qnaEnabled": true
    }
}
```

## If a report is later added (out of scope for this agent, noted for completeness)

```text
[ProjectName]/
├── [ModelName].SemanticModel/
│   └── ...(as above)
├── [ModelName].Report/
│   ├── definition.pbir
│   └── definition/  (PBIR format)
└── [ModelName].pbip
```

`definition.pbir`, targeting a local semantic model folder:
```json
{
  "$schema": "https://developer.microsoft.com/json-schemas/fabric/item/report/definitionProperties/2.0.0/schema.json",
  "version": "4.0",
  "datasetReference": {
    "byPath": { "path": "../[ModelName].SemanticModel" }
  }
}
```

Use forward slashes in `byPath`; only relative paths are supported.

## Table file naming

One `.tmdl` file per table, named to match the table's **Business Name** (quoted if it contains a space), placed under `definition/tables/`. This matches the file naming Power BI Desktop itself generates when a PBIP project is saved from the UI.
