# KPI, Benchmark, and Forecast Standards

> Consolidated 2026-10-26 from *AI-Powered Analytics Delivery Framework* Parts 3–4, *Executive Analytics Framework Playbook* Parts 2–3, and *Enterprise Analytics Standards & Governance Playbook* Parts 2 & 4 (KPI Governance, Forecast Governance sections). All three sources defined the same KPI/benchmark/forecast mechanics almost verbatim; this file merges them into one standard and keeps the governance layer (registry, lifecycle, retirement) that only the Enterprise Playbook had.

## What a KPI is

A KPI is not a number — it's a measurement designed to support a decision. "Average Annual Volume: 13,374" is a poor KPI (no context, no benchmark, no action, no decision). "Program Growth Since 2006: +144.6%" is a strong one (answers "how much has the program grown," carries history and comparison).

## The six KPI components

Every KPI should document: (1) **Business Question** it answers, (2) **Metric/Calculation** — the formula, (3) **Benchmark** — the comparison source, (4) **Threshold** — Green/Yellow/Red (or the BRD's approved neutral equivalent — see caveat below), (5) **Action** — what happens if it moves, (6) implicit sixth: **Owner**.

**KPI Validation Test** — every KPI must pass all five:
1. What business question does this KPI answer?
2. What action could this KPI trigger?
3. What benchmark provides context?
4. What threshold determines success or concern?
5. Can the user understand this KPI in under 5 seconds?

**KPI Layering Strategy** — a mature executive page typically stacks: Historical (how large is the program?) → Growth (how much have we grown?) → Current Performance (what happened most recently?) → Year-to-Date (how is the year progressing?) → Forecast (what should we expect next?).

## ⚠ Threshold/status language — BRD-gated, not automatic

This framework's default pattern is Green/Yellow/Red with "On Target"/"Action Required" status labels (see the KPI Example below). **That pattern is only valid when the confirmed BRD actually authorizes performance-style status labels.** The Phase 2 BRD (`BRD_Foster_Centre_Animal_Type_Utilization_v0.2.html` §13.1) explicitly prohibits exactly this: *"Do not use On Target, Action Required, performance scores, or resource-allocation recommendations... Use neutral labels such as Higher utilization, Lower utilization, and business-reviewed Material change."* Before applying any threshold/status design from this file, check the BRD's own interpretation boundary (§01/§02's "Interpretation boundary" callout, or equivalent) — a descriptive-only BRD gets neutral variance language, never a traffic-light verdict.

### KPI Governance (registry, lifecycle, retirement)

Every KPI is an enterprise asset, not something that lives only inside one dashboard. Document in a registry: KPI Name, Business Question, Business Definition, Calculation, Benchmark, Thresholds, Business Owner, Data Owner, Review Frequency.

**Lifecycle:** Create → Approve → Publish → Monitor → Review → Retire. **Retirement rule:** retire a KPI when it no longer supports a decision, no longer has a business owner, or no longer aligns with strategy. Review cadence: monthly for operational KPIs, quarterly for strategic ones, annually to retire obsolete ones and re-validate thresholds.

### Metric standardization / single source of truth

A metric has exactly one approved definition, enforced everywhere it's reused. For every metric, fix: System of Record, Approved Measure (name), Approved Formula — document once, reuse everywhere. Never permit two reports to silently diverge on the same metric name with different formulas — this is the exact failure mode this session already caught once (Phase 2's draft BRD redefining "% Animals in Foster" with a different denominator than Phase 1's measure of the same name).

## Benchmark Design

A benchmark should be Objective (data-based, not opinion), Repeatable, Explainable, and Governable.

**Default method — Historical Average Benchmark:**
```text
Benchmark = Average(past N periods of the same calendar point)
```
Worked example (August benchmark from 5 years of August values 9,285 / 9,473 / 9,636 / 9,417 / 9,362 → average 9,435).

**Benchmark hierarchy (strongest to weakest):** Historical Benchmark (most common) → Prior Year Comparison (good supplement) → Strategic Target (only when formally approved) → Individual Opinions (avoid).

**Variance formula:**
```text
Variance % = (Actual − Benchmark) ÷ Benchmark
```

**Threshold bands (default, subject to the BRD-gated caveat above):** Green = within ±10%, continue monitoring. Yellow = 10–15%, review trends. Red = beyond 15%, initiate investigation. Document the Definition, Formula, and Business Response for every threshold.

> **Gap this project's statistical research filled:** this file's only benchmark method is a flat multi-year average, and its only comparison is a single aggregate rate. It has no method for comparing *across dimensional groups* (e.g. centres) while accounting for population-mix differences, no concentration measure, and no outlier/change-point method beyond a visual read of a line chart. For anything beyond a single time-series KPI, see `diagnostic-method-tiering.md` — direct/indirect standardization, Kitagawa decomposition, seasonal-baseline residuals, and the rest of the Must/Prefer/Avoid table extend (not replace) this benchmark framework.

## Forecasting

**Forecasting selection questions:** is the process stable? Is seasonality predictable? Is there enough history? Does the audience need transparency over precision?

**Method hierarchy:**
1. **Historical Ratio Forecasting** — best for stable, seasonal, executive-reporting contexts. Easy to explain, validate, govern. This project's chosen method.
2. **Trend Forecasting** (linear trend, moving average, growth models) — for long-term directional planning.
3. **Advanced Statistical Forecasting** (ARIMA, Prophet, exponential smoothing, ML) — only when business value exceeds the added complexity; needs a dedicated analytical team.

**Historical Ratio Forecasting, step by step:**
1. Completion Rate = YTD Value ÷ Full Year Value, computed per historical year.
2. Historical Average Completion Rate = average of those rates across the lookback window (worked example: 87.7/87.9/88.0/90.1/89.0% → 88.5%).
3. Current Forecast = Current YTD ÷ Historical Average Completion Rate.

**Forecast validation (backtesting):** pretend you're standing in a past year, generate the forecast from what was known then, compare to what actually happened.
```text
Error % = (Forecast − Actual) ÷ Actual
Average Absolute Error = mean(|Error| across all backtested years)
```
This project's validated error: ≈ ±0.9%. A forecast without this step is an opinion, not a tested methodology.

**Forecast governance — every forecast documents:** Method, Inputs, Assumptions (e.g. "seasonality remains stable"), Validation (historical error), Limitations (e.g. "significant operational changes may reduce accuracy").

> **Second gap this project's research filled:** Historical Ratio Forecasting (and the Trend/Advanced tiers above it) is one legitimate choice for a single time series, but it says nothing about *diagnosing why* a number moved, or about seasonal baselines more rigorous than a flat 5-year average (e.g. surgically excluding a known anomalous year, per the UK ONS excess-mortality precedent in `diagnostic-method-tiering.md`). Forecasting projects a number forward; diagnosis explains a number that already happened — this skill needs both, and only had the former until this file's sibling was added.

## Executive Insight Development

Every page needs a conclusion, in this structure: **Observation** (what happened?) → **Interpretation** (why?) → **Impact** (why does it matter?) → **Action** (what should users do?).

Weak insight: "2026 Forecast = 12,800." Strong insight: "Historical Ratio Forecasting projects approximately 12,800 fostered animals in 2026. This is nearly identical to 2025 actual performance (12,778), suggesting the Foster Program remains stable... Historical backtesting produced an average forecast error of ±0.9%, supporting confidence in the projected outcome." The strong version states the number, the comparison, the method's track record, and the implication — never just the number.

## Governance principle (applies to every KPI, benchmark, and forecast in this file)

Every number must survive: *"Where did this come from? How was it calculated? Can another analyst reproduce it?"* If the answer to the last question is no, document it before publication — not after.
