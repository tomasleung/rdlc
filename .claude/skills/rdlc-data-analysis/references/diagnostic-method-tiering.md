# Diagnostic Method Tiering (Must / Prefer / Avoid)

> New 2026-10-26, distilled from `reports/Diagnostic BI statistical methods.md` (full deep-research report with citations — read that file for sourcing). This is the one piece of this skill's reference material with no precedent in the original corpus: the three legacy framework docs cover a single time-series KPI (one benchmark method, one forecast method) but have nothing for comparing *across* dimensional groups (Centre, Region, Animal Type), which is exactly what a BRD like Phase 2's adds. Use this file whenever an Insight Specification needs to name a diagnostic method, not just a benchmark.

## How to use this file

Every insight that compares groups, explains a change, flags an anomaly, or checks robustness should name a method from the table below, tagged with its tier:
- **Must** — self-service DAX/Power BI buildable, no specialist tooling required. Default choice.
- **Prefer** — feasible, but only under a stated precondition (e.g. a pre-named break date, reliable per-cell rates). Name the precondition explicitly in the Insight Specification.
- **Avoid** — needs R/Python or specialist statistical tooling (CUSUM, PELT, Bayesian changepoint, full Shapley/SHAP, STL). Never propose these as self-service; name them as a specialist-escalation item instead, or default to their Must-tier sibling.

A method is a **signal to investigate, never proof of a cause**. Every flagged result carries that caveat, stated plainly, not implied.

## Reference table

| Business question pattern | Method | Tier | What it actually answers | Key caveat |
|---|---|---|---|---|
| Why did the overall rate change — mix shift or within-group behavior? | Kitagawa / Das Gupta additive rate decomposition | **Must** | Splits a rate change into a within-group "rate effect" + population "mix effect" that sum exactly to the total change | Groups must be mutually exclusive and exhaustive each period; breaks for non-additive distinct counts (fix the grain first) |
| What's driving the change — rank top contributors | Naive contribution ratio / PVM bridge-waterfall | **Must**, with interaction/residual term | Allocates an additive change across named drivers in one chart | Divide by the prior-period *level*, not the change itself (avoids near-zero/sign-flip); invalid for non-additive distinct counts |
| Provably fair split across many interacting drivers | Shapley value / SHAP | **Avoid** | Unique order-independent attribution | For 2–3 drivers it equals Kitagawa — build that instead; exact cost grows combinatorially past ~4–5 drivers |
| Is Centre/Region X genuinely different, or just a different population mix? | Direct standardization | **Must**, if every group has reliable category-specific rates | A common-mix-adjusted, directly comparable rate per group | Thin volume in any cell corrupts the weighted average |
| Same question, small/sparse groups | Indirect standardization (expected count, O/E ratio) | **Prefer** | Actual-vs-expected index per group, using one shared reference rate | Ratios from different groups are NOT directly comparable to each other |
| One headline number on concentration | Top-N share / cumulative Pareto curve | **Must** | States what share of activity the top N groups account for | Arbitrary N; says nothing about the shape of the rest |
| Is the network becoming more/less concentrated over time? | HHI (sum of squared shares) | **Prefer**, analyst-facing trend only | A single trackable concentration index | Mechanically falls as group count rises; antitrust bands (1,800/100-pt) are a legal threshold, never a business verdict |
| Full shape of imbalance across all groups | Gini / Lorenz curve | **Prefer**, with caution | Visualizes inequality across the whole distribution | Downward-biased with few groups (relevant at ~32 centres); too abstract as a sole stakeholder headline |
| Did this period/group fall outside its normal seasonal pattern? | Seasonal-baseline standardized residual (same-period multi-year mean/median ± SD or MAD) + SPC rules | **Must** | Flags a deviation from the group's own seasonal expectation | Use MAD over SD when the baseline history is short or may itself contain anomalies |
| Same question, naive version | Global IQR fence on raw seasonal series | **Avoid** | Nothing useful — calibrated for non-seasonal data | Will flag every normal peak month and miss real off-season anomalies |
| Did the trend level genuinely and sustainedly shift? | CUSUM / Bayesian changepoint (BOCPD) / PELT | **Avoid** | Formal sustained-shift detection or optimal segmentation | No native DAX equivalent; needs R/Python and parameter tuning |
| Did the metric shift after one specific known date? | Chow test | **Prefer**, only with an analyst-nominated date | Tests whether a relationship differs before/after a hypothesized break | Cannot discover an unknown break date — that needs a specialist method |
| Does this finding survive dropping the top contributor or an anomalous period? | Leave-one-out robustness check | **Must** | Tests whether one unit is driving the conclusion | Call it this, not "jackknife" (a different, stronger formal claim); exclusions must be pre-specified, never chosen after seeing they help |
| Is this comparison reliable given low volume? | Minimum-volume / dominance floor with a flagged intermediate band | **Must** | States plainly whether a cell has enough volume to trust | No universal threshold — document the project's own floor |
| How does this period compare to normal? | Year-over-year comparison | **Must** | Simplest, most business-legible seasonal comparison | Lags the true trend; conflates trend with base-year anomaly |
| Same question, more robust | Multi-year seasonal-average baseline, with known-anomaly exclusion | **Must/Prefer** | Smooths single-year noise into an expected seasonal value | A structural break inside the window must be surgically excluded and disclosed, never silently averaged in |
| Same question, highest rigor | STL decomposition | **Avoid** | Statistically cleanest trend/seasonal/remainder split | No DAX equivalent; seasonally-adjusted output can be misread as a turning point by an untrained reader |

## Relationship to the rest of this skill's references

- `statistical-layer-mapping.md` covers a different question shape this file doesn't: is X related to Y, what's driving it, how sure are we (correlation, cohort analysis, regression, confidence intervals). Use that file for those questions; use this one for dimensional comparison and anomaly detection. Don't duplicate a method across both files — `statistical-layer-mapping.md`'s own Overlap table says which file governs which overlapping term.
- `kpi-benchmark-forecast-standards.md`'s single "5-Year Historical Average" benchmark and "Historical Ratio Forecasting" method are each one row of this table (multi-year seasonal baseline; a Level-1 forecast method) — this file extends that framework to dimensional comparisons and anomaly detection, it doesn't replace it.
- Every row's method name belongs in the **Diagnostic Method** field of the extended KPI Specification Template in `reusable-templates.md`.
- Full citations, worked formulas, and the reasoning behind each tier are in `reports/Diagnostic BI statistical methods.md` at the project root.
