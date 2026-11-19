# Statistical Layer Mapping (Data → Signal → Diagnostic → Driver → Insight)

> New 2026-11-19. Complements `diagnostic-method-tiering.md`, which covers *dimensional comparison and anomaly detection* (Centre/Region/Type-style breakdowns). This file covers a different, equally common question shape this skill had no coverage for at all: "are two measures related," "what's driving a change," "how sure are we." Same Must/Prefer/Avoid discipline, same citation rule — nothing here overrides `diagnostic-method-tiering.md` where the two overlap; see the Overlap table below.

## How to use this file

Work top to bottom. Each layer answers a different kind of business question and only makes sense once the layer above it is already answered — don't jump to Driver Discovery before Signal Detection has actually flagged something worth explaining.

- **Must** — self-service DAX/Power BI buildable, default choice.
- **Prefer** — feasible only under a stated precondition; name the precondition explicitly in the Insight Specification.
- **Avoid** — needs R/Python or specialist tooling. Never propose as self-service; name as a specialist-escalation item.

## Layer 1 — Data: "What happened?"

| Method | Tier | What it answers | Caveat |
|---|---|---|---|
| Descriptive statistics (count, sum, average, rate) | Must | The plain number itself | Not an insight on its own — every later layer builds on this, none replace it |
| KPI reporting | Must | The governed measure, as defined in the BRD | Must use the BRD's own formula, never a re-derived one (rule 3) |

## Layer 2 — Signal Detection: "Did something unusual happen?"

| Method | Tier | What it answers | Caveat |
|---|---|---|---|
| Threshold monitoring (fixed or relative) | Must | Did a measure cross a pre-agreed line | The threshold itself needs BRD authority (rule 6) — this file doesn't grant it |
| Outlier detection (IQR/z-score) on **non-seasonal** data | Must | Is this single value unusual against the rest of the series | For seasonal series, use the seasonal-baseline residual row in `diagnostic-method-tiering.md` instead — a flat IQR fence on seasonal data is that file's explicit **Avoid** row |
| Normal approximation / standardized residual | Must | Same question, with a formal distance-from-expected measure | This *is* `diagnostic-method-tiering.md`'s "seasonal-baseline standardized residual + SPC" row when the data is seasonal — don't duplicate, just cite it |
| Trend-break detection (did the level shift) | Avoid, unless pre-named | Formal sustained-shift detection | Same method as `diagnostic-method-tiering.md`'s CUSUM/PELT/Bayesian-changepoint row (Avoid) and Chow-test row (Prefer, only with an analyst-nominated break date) — cite those, don't re-specify here |

## Layer 3 — Diagnostic Analysis: "What is related to it?"

| Method | Tier | What it answers | Caveat |
|---|---|---|---|
| Correlation (Pearson/Spearman) | Must | Whether two measures move together, and how strongly | **Correlation is not causation, and must never be presented as one** (rule 7). Low-volume cells distort it the same way they distort any rate — state the sample size alongside the coefficient. |
| Cohort analysis | Must | Whether a named subgroup (defined by a BRD-confirmed dimension) behaves differently from the rest | The cohort definition must come from a BRD-confirmed Driver/dimension (rule 4) — never an ad-hoc split invented for this one chart |
| Segmentation | Must | Same as cohort analysis, using a BRD-confirmed dimension as the cut | Functionally the same governance rule as above — this is not a separate escape hatch for an unconfirmed dimension |

## Layer 4 — Driver Discovery: "What is driving it?"

| Method | Tier | What it answers | Caveat |
|---|---|---|---|
| Simple linear regression (one predictor, visual trendline) | Prefer, single predictor only | How strongly one named, BRD-confirmed variable moves with the outcome | Power BI's built-in scatter-chart trendline is the self-service ceiling. The moment a second predictor or a non-linear fit is needed, this becomes Avoid. |
| Multivariate regression | Avoid | Joint effect of several predictors | No native DAX equivalent; needs R/Python. Name as specialist-escalation. |
| Feature importance / driver ranking from a trained model | Avoid | A model's internal ranking of predictor strength | Needs ML tooling outside self-service BI. Never approximate this with an un-modeled, analyst-picked ranking and present it as equivalent. |
| Candidate-driver screening via Layer 3's correlation strength | Must, but explicitly provisional | Which already-confirmed Drivers are *worth investigating* further | This is the self-service substitute for formal driver ranking — it produces a short list to investigate, never a settled, final ranking. Must draw only from Drivers already confirmed in the BRD (rule 4); never introduces a new candidate driver that isn't already in scope. |

## Layer 5 — Decision Insight: "What should we focus on?"

This layer translates Layer 3/4's output into business language (`eda-and-business-questions.md` Step 5), not a new statistical method. The only rule that matters here: state what's worth investigating, never what caused what.

- **Permitted phrasing:** "X and Y move together, and X is worth investigating as a contributing factor."
- **Prohibited phrasing:** "X is the primary driver/constraint/cause of Y" — this is a causal claim from a descriptive or diagnostic finding, which rule 7 explicitly forbids regardless of how strong the correlation or regression fit looks.

## Layer 6 — Action: out of scope for this skill

A recommended action ("increase recruitment," "expand capacity at Centre X") is a business decision, not an analysis output. This skill's role boundary stops at Decision Insight (Layer 5) — the actual decision belongs to the BRD's named Decision Owner, using the evidence this skill assembled. Do not draft a recommended action as part of an Insight Specification entry; if a consumer needs one, redirect to the Decision Supported section of the BRD and the business/decision owner, not this skill.

## Overlap with `diagnostic-method-tiering.md` — do not duplicate

| This file's term | Already covered as | Rule |
|---|---|---|
| Normal approximation (seasonal) | Seasonal-baseline standardized residual + SPC | Cite the existing row, don't restate it |
| Trend-break detection | CUSUM/PELT/Bayesian changepoint (Avoid); Chow test (Prefer, pre-named date) | Cite the existing rows |
| Variance analysis ("why is actual different from expected") | Naive contribution ratio / PVM bridge-waterfall | Cite the existing row |
| Forecasting | Fully specified in `kpi-benchmark-forecast-standards.md` (Historical Ratio Forecasting, method hierarchy, backtesting) | Use that file, not this one |

## Relationship to the rest of this skill's references

- Use this file for "are they related / what's driving it / how sure are we" questions. Use `diagnostic-method-tiering.md` for "is this group different, is this concentrated, did this change" questions. A single Insight Specification entry may cite either file, never invent a method absent from both.
- Every method name here belongs in the **Diagnostic Method** field of the extended KPI Specification Template in `reusable-templates.md`, same as a method from `diagnostic-method-tiering.md`.
