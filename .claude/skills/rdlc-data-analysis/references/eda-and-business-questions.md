# EDA and Business Question Discovery

> Consolidated 2026-10-26 from *AI-Powered Analytics Delivery Framework* Part 2, *Executive Analytics Framework Playbook* Part 1–2, and *Enterprise Analytics Standards & Governance Playbook* Part 1 (Principles 1–2). The three sources covered this ground almost identically (same framework, same worked examples); this file keeps the one combined version instead of three near-duplicates. Nothing substantive was dropped — only repetition.

## Why EDA comes first

Most failed analytics projects follow `Data → Dashboard`. The organizing principle of this whole skill is `Data → EDA → Patterns → Insights → Questions → KPIs → Visuals → Dashboard` — the dashboard is the *last* step, not the first. Skipping straight to visuals produces KPI overload, weak storytelling, and dashboards nobody uses to decide anything.

**In this project's actual pipeline**, EDA against live data happens at a later, execution-time stage — this skill (Analysis Design) runs on a *confirmed BRD*, before any data is queried. Treat the EDA steps below as the questions this stage must already have governed answers to (from the BRD), not as live data profiling this stage performs itself.

## EDA workflow

**Step 1 — Understand the data.** Before any calculation, document: Business Context, Intended Audience, Decisions To Support, and — critically — **Data Grain** ("one row represents ___"). Never proceed until the grain is stated in one sentence.

**Step 2 — Profile the data.** Time range, metrics, dimensions, missing values, quality issues (duplicates, unexpected spikes/drops). For this project, this is the BRD's §07 Analytical Approach and §09 Data Sources, read, not re-derived.

**Step 3 — Challenge assumptions.** Never assume the business narrative is correct. Worked example from this project: the assumption "the Foster Program is growing" did not survive EDA — the data showed rapid growth 2006–2010 followed by a stable, mature operating range. The corrected narrative, not the original assumption, drove the KPI design.

**Step 4 — Identify patterns**, across four lenses:
- *Trend* — growing / declining / stable?
- *Seasonality* — recurring peaks/lows, repeating patterns?
- *Variance* — actual vs. benchmark, vs. prior year, vs. historical average?
- *Forecastability* — stable enough process, predictable seasonality, enough history to project?

**Step 5 — Convert findings into business statements.** A finding like "Long-Term CAGR = 4.8%" is not useful to an executive. Translate it: "The Foster Program experienced significant growth before transitioning into a mature operating model." Business language outranks statistical language every time.

## Business Question Discovery

After EDA: build questions, not visuals. `Dashboard = Decision Support System` only when every visual, KPI, and page traces to a specific question; otherwise it's just `Dashboard = Collection of Charts`.

**Discovery process:** for every insight, ask "What question does this insight answer?" (Insight: "Growth occurred primarily before 2010" → Question: "Is the Program Growing?")

**The Question Hierarchy** — a strong report usually contains all four layers:
| Layer | Example |
|---|---|
| Strategic | Where are we headed? How has the program evolved? |
| Executive | Is the Program Growing? Is There Seasonality? |
| Operational | When should resources be allocated? Are we tracking against benchmark? |
| Tactical | Which month performed below expectation? Which segment needs investigation? |

**AI validation questions**, to ask before any dashboard design starts: What is the actual story? What assumptions appear incorrect? What business questions emerge naturally from the data? What should leadership understand? What should operational teams do? What evidence supports each conclusion? If these can't be answered yet, continue analysis — don't start design.

## Business Question Template

```text
Question:            [plain-language business question]
Executive Answer:     [shortest possible answer]
Evidence:             [KPIs / charts / benchmarks / forecasts that support the answer]
Executive Insight:    [why this matters, what action it implies]
```

## Page Design Framework (Question-Driven Dashboard Model)

Every page, every visual, every KPI must support one specific question. For each page, define:
- **Question** — e.g. "How has the program evolved?"
- **Executive Answer** — the shortest true answer, stated before detailed evidence
- **Evidence** — charts, KPIs, benchmarks, forecasts
- **Executive Insight** — the business conclusion

## Scope-drift guardrail (project-specific addition)

The worked examples throughout this file and its siblings are single-dimension, time-only (this is literally what Phase 1 built — see `foster-program-dashboard-functional-specification.md` for the actual delivered data model, which has no Centre/Region/Animal Type dimension at all). **When a confirmed BRD adds dimensional breakdowns** (as the Phase 2 BRD does — Centre, Region, Animal Type Family), the Business Question Template and Page Design Framework above still apply, but the *Drivers* (the dimensions each question is sliced by) must come only from that BRD's own confirmed in-scope dimensions — never copied from this file's time-only examples, and never copied from an unrelated project's dimension list. See `diagnostic-method-tiering.md` for which statistical method backs a dimensional comparison once the Driver itself is confirmed.

## Key lessons

1. EDA comes before visualization.
2. Insights come before KPIs.
3. Questions come before charts.
4. Business language outranks statistical language.
5. A dashboard should answer questions, not display data.
