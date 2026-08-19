# RDLC OS — Agent Ecosystem Status Tracker

*Last updated: August 19, 2026 (post live Verify Mode — dual independent check)*

*Temporary home: this file tracks the RDLC OS agent ecosystem as a whole, not this specific project. It currently lives inside `foster-analysis/` for convenience while the ecosystem is still being actively built. It will move to a dedicated skills repo once one exists — do not duplicate it into other project folders (e.g., a future `live-capacity-analysis/`) in the meantime; there should be exactly one copy.*

---

## 1. Vision Recap

RDLC OS decomposes BI development into a chain of **single-responsibility sub-agents**, each owning one pipeline stage, each with an explicit input/output artifact contract to the next. Every agent is scoped tightly (QA-only, generation-only, build-only) with the human orchestrator retaining decision authority at every gate.

```
PM + Senior Data Analyst     Data Architect + Data Modeler   Data Prototyping Agent   Implementation Agent
   (NOT YET BUILT)        →   rdlc-coach-semantic-model   →  rdlc-mock-data-      →     rdlc-tmdl-build-
                               ✅ COMPLETE                     generator                  agent
   Produces:                                                  ✅ COMPLETE                ✅ COMPLETE
   - BRD                      Produces:
   - Discovery Framework      - Confirmed Table              Produces:                  Produces:
   - Draft Table Definitions    Definitions doc               - Referentially-valid       - Built PBIP/TMDL
     (rough, not Kimball-       (grain-correct,                 mock CSVs                   semantic model
     clean)                     Kimball/Fabric-aligned,         (Prototype mode)            (Prototype or
                                 versioned)                                                  Production mode)
```

**Status: 3 of 4 identified agents complete.** The upstream Senior Data Analyst agent remains the only unbuilt link in the chain.

---

## 2. Agent Status Summary

| Agent | Role Pairing | Status | Notes |
|---|---|---|---|
| **PM + Senior Data Analyst** | Upstream | 🔲 Not started | Produces BRD, Discovery Framework, draft Table Definitions. Output contract is fully specified — mirrors `rdlc-coach-semantic-model`'s input contract exactly. |
| **`rdlc-coach-semantic-model`** | Data Architect + Data Modeler | ✅ **Complete, v1.0** | QA/coaching agent. Grills the human against Kimball + MS Fabric/Power BI standards. Never authors from scratch, never builds. |
| **`rdlc-mock-data-generator`** | Data Prototyping Agent | ✅ **Complete, v1.0** | Turns a confirmed Table Definitions doc into referentially-valid mock CSVs. Grills the human for per-table volumes; derives fact-table size from grain + dimension volumes rather than asking directly. |
| **`rdlc-tmdl-build-agent`** | Implementation Agent (Step 13) | ✅ **Complete, v1.1 — real-world validated via Claude Code** | Hand-authors PBIP/TMDL files directly on disk — MCP is a narrow secondary mode (validation / live Fabric ID discovery only), never the primary authoring loop. Run for real on 2026-08-18: built the Foster Analysis model, found and fixed 2 real TMDL bugs via actual Power BI Desktop testing, confirmed 3/3 core KPIs match predicted values exactly. |

---

## 3. What's Been Completed

### 3.1 All three skill packages (GitHub-ready, currently embedded per-project in `.claude/skills/`)

Each follows the identical structure:
```
<skill-name>/
├── SKILL.md              # Role, non-negotiable gates, input contract, workflow
├── README.md
└── references/
    ├── <standards files>.md    # Reasoning anchors — soft guidance, applied contextually
    ├── <format spec>.json      # Hard, deterministic output constraints (where applicable)
    └── gold-example/           # Real, worked output — not a hypothetical
```

| Skill | Non-negotiable gate(s) | Gold example |
|---|---|---|
| `rdlc-coach-semantic-model` | NO CONFIRMED GRAIN → NO FACT TABLE; NO DRAFT → NO REVIEW | `TABLE_DEFINITIONS_FOSTER_v1_2.docx` |
| `rdlc-mock-data-generator` | NO CONFIRMED TABLE DEFINITIONS → NO MOCK DATA; referential integrity mandatory | Foster Analysis CSVs (32 centres, 40 animals, 34→8 intake types, 600 fact rows) |
| `rdlc-tmdl-build-agent` | NO CONFIRMED TABLE DEFINITIONS → NO BUILD; static self-validation mandatory before "done" | `Foster Analysis.SemanticModel/` — 5 tables, 3 measures, 4 relationships |

### 3.2 Key cross-cutting design decisions locked in across all three

- **Grilling protocol** (mattpocock/skills, embedded verbatim + upgraded to rounds/frontier/`❓ Qn / ➡️` format) is the shared interaction pattern for every agent that needs human decisions, not just the reviewer.
- **Deferral handling**: "I don't know, use your recommendation" is valid but is flagged as *Assumed, pending validation* — never given the same confidence as a direct answer.
- **Technical grounding sourced from real, verified documents** — not paraphrased fragments. Microsoft's `skills-for-fabric` `semantic-model-authoring` skill (modeling guidelines, naming conventions, TMDL syntax, AI-readiness, Direct Lake, PBIP structure) and the official Tabular Editor Best Practice Analyzer rule set (71 rules) were both read in full before being synthesized into reference files.
- **MS-wins conflict resolution**, stated explicitly wherever Microsoft's guidance and Tabular Editor's rules disagree (documented case: `isKey` on dimension primary keys — MS says don't set it, BPA says do; MS governs).
- **Two-name convention**: every table/column has a Technical Name (Fabric/ETL-facing, `sourceColumn` mapping) and a Business Name (Power BI-facing, human-readable) — introduced in Table Definitions v1.2, carried through mock data and the TMDL build.
- **Provenance tracking**: every reference file sourced from an external document has a "Source Provenance" table (URL + last-verified date), so future updates can re-fetch and diff rather than silently trusting stale content.
- **Self-validation before declaring done**, applied consistently: the reviewer visually re-renders its own output doc; the mock data generator re-parses its own CSVs for referential integrity; the build agent runs a static TMDL checklist. All three caught real defects during their own construction, not just in theory — see §3.4.

### 3.3 Real output produced using this chain (BC SPCA Foster Analysis, Phase 1)

| Artifact | Status |
|---|---|
| Table Definitions | v1.0 (rough draft) → v1.1 (grain/key/type corrections) → v1.2 (Fabric naming/build-readiness pass) → v1.3 (Species Group added, Data-Owner-confirmed) |
| Mock data | 5 CSVs, 731-day contiguous date range, 32 centres/5 regions, 40 animals, 34 intake types/8 groups, 600 fact rows, seeded (seed=42) for reproducibility. Realistic (non-uniform) distributions: 40% foster rate overall but 75% for Neonates; 54% of intakes in kitten season (Jun-Aug) |
| PBIP/TMDL build | `Foster Analysis.SemanticModel/` — complete, statically validated, not yet opened in a live Power BI Desktop instance (pending) |

### 3.4 Real defects caught by each agent's own self-validation (worth remembering as proof the gates work, not just documentation)

- `rdlc-mock-data-generator`'s gold example: first draft had 8 orphan foreign keys (fact rows referencing dates not present in a too-small `Dim_Date`) — caught by the mandatory referential integrity check, fixed before presenting.
- `rdlc-tmdl-build-agent`'s gold example (static validation, pre-Claude Code): a measure and its own hidden base column shared the identical name (`Intake Count`) — a real Power BI anti-pattern per `modeling-guidelines.md`. Caught by the static validation checklist, fixed. Same pass also caught 8 visible dimension columns missing required `///` descriptions.

### 3.5 Real-world Claude Code build — first live test of any RDLC OS agent (2026-08-18)

`rdlc-tmdl-build-agent` was run for real, via Claude Code, against an actual empty local Power BI project — not just designed and statically validated. Two real bugs surfaced that static validation (run against text alone, no live engine) could not have caught:

| Bug | Symptom | Root Cause | Fixed In |
|---|---|---|---|
| Orphan `///` comment | `InvalidLineType: Empty!` on open | A `///` block not immediately attached to an object, followed by a blank line | `tmdl-syntax-guide.md`, `static-validation-checklist.md`, gold example |
| `description` on a relationship | `Property 'description' is unknown...` on open | Relationships don't support a description property in TOM, unlike tables/columns/measures | `tmdl-syntax-guide.md`, `static-validation-checklist.md`, gold example |

Both fixes are now permanent in the skill's reference material — not one-off patches to a single build. Full detail in `foster-analysis/.claude/skills/rdlc-tmdl-build-agent/usage/CHANGELOG.md`.

**After both fixes**, the model opened cleanly and a predict-then-confirm KPI check (methodology and filled-in result both in `usage/` — `TDD-VERIFICATION.md` and `FOSTER-ANALYSIS-VALIDATION.md`) matched exactly: Intake Count 600/600, Animals Ever Fostered 241/241, Foster Placement Rate 40.17% against a ≈40% prediction.

**New pattern established**: skills actually run via Claude Code (as opposed to conversationally) now get a `usage/` subfolder — `SOP.md` (human operating procedure), `CHANGELOG.md` (real bugs + fixes), `TDD-VERIFICATION.md` (reusable predict-then-confirm methodology), and filled-in result files named per-project (e.g. `FOSTER-ANALYSIS-VALIDATION.md`) sitting alongside the methodology they instantiate. This is distinct from `references/`, which remains agent-facing only. Not yet promoted to a project-root or cross-skill pattern — evidence from one skill isn't enough to generalize yet (see Open Items).

### 3.6 Confirmed project folder structure (per-project, not shared)

```
<project-name>/                      # One standalone project per data capability, e.g. foster-analysis/
├── README.md                        # Full agent-chain narrative for this project
├── CLAUDE.md                        # Short, operational — loaded automatically by Claude Code
├── .claude/skills/                  # All 3 RDLC OS skills, embedded (not yet a separate shared repo)
│   └── rdlc-tmdl-build-agent/
│       ├── references/              # Agent-facing: how Claude Code should build
│       └── usage/                   # NEW — human-facing: SOP, CHANGELOG, TDD methodology + filled-in results
├── 01-inputs/                       # BRD, Discovery Framework, rough draft Table Definitions (v1.0)
├── 02-table-definitions/            # Only agent-reviewed/confirmed versions (v1.1+)
├── 03-mock-data/                    # CSV fixtures from rdlc-mock-data-generator
└── 04-powerbi-project/              # PBIP/TMDL output from rdlc-tmdl-build-agent
```

**On `usage/` folder placement**: kept skill-scoped (inside `rdlc-tmdl-build-agent/`), not project-root, and validation *results* live alongside the *methodology* they instantiate (`FOSTER-ANALYSIS-VALIDATION.md` next to `TDD-VERIFICATION.md`) rather than in a separate numbered folder. A project-root validation folder would only make sense once multiple skills produce their own validation reports — speculative right now, not evidenced. Revisit if that happens.

**Rule for `01` vs `02`**: `01` = everything fed *into* `rdlc-coach-semantic-model`; `02` = everything it *produced*. Clean, reusable for every future project without re-litigating the split each time.

**Current stance on skills location**: embedded per-project in `.claude/skills/`, copied rather than shared, since the ecosystem is still actively changing. Deliberate, temporary — not yet worth the sync overhead of a separate skills repo (git submodule or versioned-zip pull) until the skills stabilize. Revisit once a second project (e.g., Live Capacity Analysis) actually starts.

### 3.7 First live Verify Mode pass — dual independent verification (2026-08-19)

Two separate live checks run back-to-back against the built Foster Analysis model:
1. **Live MCP review** — Claude Code invoked Microsoft's real `powerbi-authoring:semantic-model-authoring` skill directly (not `rdlc-tmdl-build-agent`), via `powerbi-modeling-mcp`, read-only.
2. **Manual Tabular Editor BPA** — the human operator independently ran the full 71-rule Best Practice Analyzer against the same live model.

**5 findings surfaced, all correctly triaged** — 1 real gap fixed (`isAvailableInMdx` on 11 hidden columns), 1 documentation-only gap closed (hidden-column casing exemption reasoning made explicit), 2 already-known accepted tradeoffs formally documented for the first time (String relationship keys; retained `Animal ID`/`Source Intake ID`), and 1 genuinely open question explicitly parked rather than guessed at (`isKey`'s possible AI-readiness relevance — also caught and corrected an earlier wrong claim in this session that `isKey` is "deprecated legacy," which checked out false on verification). Full detail: `usage/CHANGELOG.md`.

**Setup required to get here was substantial** and is now itself documented for reuse: `usage/MS-FABRIC-MCP-SETUP.md` captures the full real path — plugin marketplace install, the session-restart requirement for MCP registration, the `InteractiveBrowser` auth handshake, and 4 real environment issues hit and resolved along the way (chat-sandbox false negatives when checking tool versions, an unrecognized slash command, a stalled auth timeout, and the mid-session-install-doesn't-count trap).

**Both `modeling-and-ai-readiness-standards.md` and `static-validation-checklist.md` updated** to reflect all 5 findings — including adding exemption pointers to the checklist so it doesn't flag the 2 already-known tradeoffs as new defects on future runs.

---

## 4. Output Contract Now Defined for the (Not-Yet-Built) Senior Data Analyst Agent

Unchanged from before — still the immediate next build candidate. Needs to produce, per `01-inputs/`:
1. BRD — business decision, business questions, required signals
2. Discovery Framework — analytical approach, named drivers, phase scope (in/out)
3. Draft Table Definitions — rough is fine: proposed table names, rough column list, short description per table. No Kimball fluency required — that's `rdlc-coach-semantic-model`'s job.

---

## 5. Open Items / Next Steps

- [ ] Design the **PM + Senior Data Analyst** agent (upstream) — the one remaining unbuilt link
- [x] ~~Open `Foster Analysis.SemanticModel` in an actual Power BI Desktop instance~~ — done 2026-08-18, see §3.5
- [ ] Add bounded-retry rule to `SKILL.md`'s Verify Mode (max 1 corrected MCP retry, then stop and report) — agreed in principle, not yet written in. Currently only documented as manual guidance in `usage/SOP.md`.
- [x] ~~Run Verify Mode (MCP) for the first time~~ — done 2026-08-19, twice over (live MCP + manual BPA), see §3.7
- [ ] `isKey` / AI-readiness open question — parked 2026-08-19, not resolved. Revisit if Copilot/AI-agent readiness becomes a real priority; start from `isDefaultLabel`, not `isKey`.
- [ ] Date table marking / format string discrepancies (from the same review pass) — partially investigated: Date table confirmed correctly marked in a live, data-loaded Desktop session (earlier flag likely a metadata-only MCP connection limitation). Format string application not yet independently re-checked. Low priority.
- [ ] Decide whether the `usage/` subfolder pattern (SOP/CHANGELOG/TDD-VERIFICATION) should be promoted to the other two skills, or to a project/root-level pattern — currently only exists for `rdlc-tmdl-build-agent`, the only skill run via Claude Code so far. One data point isn't enough to generalize yet.
- [ ] Decide report-authoring scope: does a future agent own `.Report/` PBIP output, or does that stay entirely outside this repo structure? (Flagged, not yet decided.)
- [ ] Once skills stabilize, move them out of per-project `.claude/skills/` into a dedicated skills repo (git submodule or versioned-zip pull) — and move this tracker file there too
- [ ] BRD_Foster_Analysis_v1.0 §7.1 still needs the documented correction (remove Foster Type/Outcome/Placement Dates from Phase 1 signal list) — flagged repeatedly, not yet actioned as a source-document edit
- [ ] Species Group mapping incomplete — only Cat/Dog/Small Animal confirmed by the Data Owner; "Other" is a pending-mapping placeholder, not final
- [ ] `allowed-tools` frontmatter in each `SKILL.md` uses Claude-Code-specific syntax — still an open question whether a plain-language fallback is needed for non-tool-calling environments

---

## 6. Artifact Index

| Artifact | Location |
|---|---|
| `rdlc-coach-semantic-model/`, `rdlc-mock-data-generator/`, `rdlc-tmdl-build-agent/` | `foster-analysis/.claude/skills/` |
| Table Definitions v1.0 | `foster-analysis/01-inputs/` |
| Table Definitions v1.1–v1.3 | `foster-analysis/02-table-definitions/` |
| Mock data CSVs | `foster-analysis/03-mock-data/` |
| PBIP/TMDL build | `foster-analysis/04-powerbi-project/Foster Analysis.SemanticModel/` (validated, opens cleanly, KPIs confirmed) |
| Build usage docs (SOP, Changelog, TDD methodology, filled-in results) | `foster-analysis/.claude/skills/rdlc-tmdl-build-agent/usage/` |
| This status tracker | `foster-analysis/` root (temporary — see note at top) |
