# Analytics Governance, AI Governance, and Review Board

> Consolidated 2026-10-26 from *Enterprise Analytics Standards & Governance Playbook* Parts 1 & 4 — unique to that source; the other two framework docs don't define an AI governance layer or a review board.

## Analytics Maturity Model

| Level | Focus | Characteristics | Output |
|---|---|---|---|
| 1 | What happened? | Static reports, manual exports | Tables, spreadsheets |
| 2 | What is happening? | KPIs, interactive dashboards | Power BI reports |
| 3 | Why is this happening? What should we do? | Benchmarks, thresholds, executive insights | Executive/planning dashboards |
| 4 | What is likely to happen? | Forecasting, trend modeling, scenarios | Forecast reports |
| 5 | What decision should be made? | AI-assisted analytics, recommendations | Decision support systems |

Target state for every project in this framework: **Level 3+ (Decision Support)**, not simple reporting. This skill's own job — Analysis Design — is explicitly what moves a project from Level 2 toward Level 3: turning a confirmed BRD's KPIs into a governed diagnostic design rather than a flat KPI list.

## Enterprise analytics principles

1. Business questions first, not charts.
2. Analysis before design — `Data → EDA → Insights → Questions → KPIs → Visuals`, never reversed.
3. Decision support over reporting — every visual supports a decision, action, or planning activity.
4. Governed metrics — every metric has a definition, owner, calculation, business purpose.
5. Trust through transparency — forecasts, KPIs, benchmarks must be explainable, documented, reproducible.

**Governance principle (the test every output must pass):** *can another analyst independently reproduce this result?* If no, governance is incomplete.

## AI Governance Framework

**Approved AI use cases:** Exploratory Data Analysis (pattern detection, hypothesis generation, outlier investigation); Business Analysis (question development, narrative creation, requirements analysis); Dashboard Design (KPI design, UX review, storytelling); Documentation (functional specs, methodology docs, developer handoff packages).

**Human validation requirement:**
```text
AI Generates → Human Validates → Organization Publishes
```
AI may assist with all of the above. AI never replaces business ownership, governance approval, or human validation.

**AI auditability standard.** For critical outputs — forecasts, KPIs, executive reporting — document: Input, Prompt, Method, Validation Performed. This is the standing rule this session has followed throughout (every AI-produced BRD, measure, or analysis design in this project gets reviewed and confirmed by the human before it's treated as settled).

## Analytics Review Board (ARB)

Purpose: governance before a solution reaches production. Recommended membership and responsibilities:

| Role | Responsible for |
|---|---|
| Business Owner | Business value, decision support, strategic alignment |
| Program Owner | Operational requirements, practical usage, adoption |
| Analytics Lead | Methodology, KPIs, forecasting, statistical validity |
| Data Engineering Lead | Data quality, pipeline design, Fabric architecture |
| Power BI / Semantic Model Lead | Technical design |

**Review areas, each with its own question set:**
- *Business review* — does the solution support decisions? Does every KPI matter? Does every page add value?
- *Analytical review* — are calculations correct? Are assumptions reasonable? Are benchmarks defensible?
- *Forecast review* — has forecasting been validated? Is confidence documented?
- *UX review* — is the story understandable? Can executives understand the page quickly?
- *Governance review* — can another analyst reproduce the result?

In this RDLC OS project, `rdlc-gate-monitor` performs the structural-compliance half of this review (does each stage's artifact meet its own declared gate); the ARB's *business* and *analytical* review areas above are the human sign-off this skill's own handoff gate requires before passing an Insight Specification downstream.
