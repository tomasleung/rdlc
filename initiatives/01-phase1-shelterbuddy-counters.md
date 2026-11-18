# Initiative 01 — Foster Pathway Analytics, Phase 1 (ShelterBuddy)

**Status: CONTESTED.** `CLAUDE.md`'s old "Current State" section did not match the actual files on disk or the actual confirmed BRD. Do not trust any pre-2026-11-18 description of Phase 1 until this is resolved.

## What was previously claimed

`CLAUDE.md`/`README.md` (pre-2026-11-18) described Phase 1's core measure as `% Animals in Foster = Foster Animals ÷ Intake Animals`, built on `Fact_AnimalIntake`, with Table Definitions at `02-table-definitions/TABLE_DEFINITIONS_FOSTER_v1_3.docx` and a PBIP at `04-powerbi-project/Foster Analysis.SemanticModel/`.

## What's actually confirmed/on disk, verified 2026-11-18

- The only Phase 1 BRD in this repo is `01-inputs/BRD_Foster_Analysis_Phase1_Counters_v1_1.docx` ("Counter Scope," last updated 2026-08-31). It **narrows** Phase 1 to two raw counters — **Fostered Animals** and **Foster Placement Count** — on a *new* fact table, and explicitly **defers** the rate measure ("Foster Placement Rate %") pending Data Owner confirmation of denominator logic. All four approvals (Product Owner **Maggie Stewart**, Data Owner **Cynthia Boulter**, Data Architecture **Tomas Leung**, Decision Owner) are **Pending**.
- `01-inputs/TABLE_DEFINITIONS_FOSTER_EPISODE_ANALYTICS_v1.3.docx` (misfiled in `01-inputs/`, not `02-table-definitions/`, which only holds v1.2) implements those two counters on `Fact_FosterPlacement` + `Fact_FosterAnimalMonth` — a different table name than the BRD's own `Fact_FosterEpisode`. It self-labels as "Category 2 — Capacity & Support... separate from Phase 1 Fact_AnimalIntake" and flags its own gap: never formally reconciled with a BRD/Discovery Framework update.
- `03-mock-data/` actually contains `Dim_Date.csv`, `Fact_FosterPlacement.csv`, `Fact_FosterAnimalMonth.csv` — matching the BRD v1.1/Table-Defs-v1.3 generation, not the `Fact_AnimalIntake` generation `CLAUDE.md` described.
- `04-powerbi-project/` actually contains **three** separate PBIP folders — "Foster Dashboard," "Foster Program Historical & Forecast Baseline," and a "- Copy" of that last one — none named `Foster Analysis.SemanticModel`.

## Open thread (2026-11-18)

Tomas stated the real intended measure is `% Animal in Foster = Foster Animal ÷ Intake Animal`, with an 80% target the Data Owner wants to watch — matching the *deferred* item in BRD v1.1, not the confirmed one. Then, reviewing `BRD_Foster_Centre_Animal_Type_Utilization_v0.3.html` (initiative 02), its Appendix D cites **"Foster Phase 1 BRD, Version 2.0, October 2026"** as the authoritative source for exactly this measure — but **no v2.0 of the Phase 1 BRD exists anywhere in this repo.** Two possibilities, unresolved:
- (a) a real v2.0 exists outside this repo and just needs to be added to `01-inputs/`, which would confirm the 80%-target measure, or
- (b) v2.0 doesn't really exist yet and both the CATU BRD and the old `CLAUDE.md` were citing/assuming a confirmation that never happened.

## Report build progress (`04-powerbi-project/Foster Dashboard.Report`)

Promoted from private memory 2026-11-18 — not yet reconciled with which BRD/initiative this report actually serves, given the Phase 1 measure confusion above. Building page by page, driven from the mock `Spec/foster_program_executive_dashboard.html` and per-page specs `Spec/page1_*.md`–`page4.md` (the `.md` specs are the authoritative content/title source; the HTML mock is authoritative for visual style/nav labels).

- Page `dc20d77f60a4362e796e` "Director Summary" — new page 1, built with custom theme + top header/nav bar (dark slate-900 bar, eyebrow+title textbox, 4-tab nav pill: Director Summary/Growth Analysis/Operations Planning/Methodology). No KPI/chart content yet.
- Custom theme: `StaticResources/RegisteredResources/FosterExecutive-<guid>.json`, indigo/slate/emerald/amber/rose palette derived from the mock's Tailwind colors. Segoe UI substituted for the mock's Poppins/Lato (not installed locally).
- Nav buttons for Growth Analysis/Operations Planning/Methodology have no click-action wired yet — those target pages don't exist. Wire each once its page is built.
- Two pre-existing pages left untouched per Tomas's explicit choice: `819983355d2cb98776a0` "Foster Overview Dashboard" (11 visuals, WIP/experimental) and `9a2bb25c660e70c162b8` "Page 9" (4 visuals) — both have pre-existing validation errors (missing `nativeQueryRef`, undersized slicers, missing `queryState`) that are not to be fixed unless asked.
- **Known gotcha while editing this report:** see the Power BI Desktop save-clobbers-external-edits entry in `.claude/skills/rdlc-tmdl-build-agent/usage/CHANGELOG.md` (2026-09-29 entry) — never ask the user to save in Desktop to clear `hasUnsavedChanges` before a reload once new files have been written externally; that clobbers them.

## Known gaps (carried from `CLAUDE.md`, not yet re-verified against the v1.1/v1.3 generation)

- Species Group mapping incomplete
- Municipality-level geography deferred
- EAB/LSAI Program derivation unconfirmed

## Next

Find or create the real Phase 1 BRD v2.0 (or confirm it doesn't exist and the rate measure is still genuinely deferred), then fix the `Fact_FosterEpisode` vs. `Fact_FosterPlacement`/`Fact_FosterAnimalMonth` naming mismatch between the BRD and its Table Definitions. Do this before trusting any "Phase 1 is done" framing elsewhere, including in the report build above.
