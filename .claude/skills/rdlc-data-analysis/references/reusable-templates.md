# Reusable Templates

> Consolidated 2026-10-26 from the Appendices of *AI-Powered Analytics Delivery Framework* and *Executive Analytics Framework Playbook* (near-identical appendices in both). The **KPI Specification Template** below is the one template extended with new fields this session added — the rest are kept as originally defined.

## Executive Question Template

```text
Executive Question:    [what business question are we answering?]
Executive Answer:      [shortest true answer]
Supporting Evidence:   [KPIs / charts / benchmarks / forecasts]
Executive Insight:     [why this matters, what leaders should do]
```

## KPI Specification Template — extended

The original template had six fields (Name, Business Question, Business Meaning, Calculation, Benchmark/Threshold, Action). This session added four fields the original lacked, needed once a KPI is sliced by a dimensional driver (Centre, Region, Animal Type) rather than just time: **Population**, **Grain**, **Diagnostic Method**, and **Permitted / Prohibited Conclusion**. Use the full template for anything produced by this skill:

```text
KPI / Insight Name:
Business Question:        [plain language]
Decision Supported:       [from the confirmed BRD — never invented here]
Business Meaning:         [one sentence]
Population:                [who/what is counted — must match the BRD's KPI contract exactly]
Grain:                      [e.g. Month + Centre + Animal Type Family]
Calculation:               [formula]
Comparison / Benchmark:   [what it's measured against — see kpi-benchmark-forecast-standards.md]
Minimum Volume:            [low-volume safeguard threshold, if a rate or ranking is involved]
Diagnostic Method:         [name + tier (Must/Prefer/Avoid) from diagnostic-method-tiering.md — blank only for a plain time-series KPI that needs no dimensional method]
Threshold / Status:        [Green/Yellow/Red ONLY if the BRD's interpretation boundary allows status language; otherwise neutral variance language]
Permitted Conclusion:      [what this KPI can legitimately support saying]
Prohibited Conclusion:     [what it must NOT be used to claim — e.g. causation, performance, capacity sufficiency]
Action:                    [what happens if it moves materially]
Owner:
```

## Benchmark Specification Template

```text
Benchmark Name:       [e.g. 5-Year Historical Average]
Purpose:              [what expected performance it represents]
Data Sources:         [which periods feed it]
Formula:              [exact calculation]
Variance Formula:     (Actual − Benchmark) ÷ Benchmark
Threshold Rules:      [Green/Yellow/Red bands, if BRD-authorized]
```

## Forecast Specification Template

```text
Forecast Name:
Method:              [e.g. Historical Ratio Forecasting]
Inputs:              [e.g. YTD Activity, Historical Completion Rate]
Formula:
Assumptions:         [e.g. seasonality remains stable]
Validation:          [backtesting method, historical error]
Risks:               [what could invalidate the forecast]
```

## Visual Design Template

```text
Business Question:
Executive Answer:
Visual Type:          [line / bar / area / heatmap / KPI card / matrix]
Key Data:             [measures, dimensions, filters]
Annotations:          [peak year, lowest month, forecast point, phase boundaries...]
Executive Insight:    [one concise conclusion]
```

## Dashboard review checklist

- **Business** — does every page answer a business question? Does every KPI support a decision? Does every conclusion have evidence?
- **KPI** — understandable in 5 seconds? Context present? Benchmark present? Action defined?
- **Forecast** — validated? Explainable methodology? Accuracy documented?
- **UX** — simple? Clear story? Unnecessary detail removed? Conclusions quick to grasp?

## Project kickoff checklist (before this skill starts work)

- **Discovery** — audience identified? Decisions being made identified? Problem statement clear? Data availability known? (All of this should already be settled in the confirmed BRD — this skill verifies, it does not re-derive.)
- **Analysis** — assumptions challenged? Trends validated? Seasonality investigated? Outliers reviewed?
- **Reporting** — business questions documented? KPIs engineered? Benchmarks defined? Forecasts validated?
- **Development readiness** (downstream, not this skill's job) — specifications complete? Calculations reproducible? Can a developer build independently?

## Recommended project deliverables

Every analytics engagement should eventually produce: Business Questions, EDA Findings, KPI Specifications, Benchmark Specifications, Forecast Methodology, Dashboard/Report Specification, Developer Handoff, Governance Package. This skill is specifically responsible for the first three (as an Analysis Design Brief + Insight Specification Register) before the BRD moves to `rdlc-coach-semantic-model`.
