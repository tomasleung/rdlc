# UX and Dashboard Storytelling Standards

> Consolidated 2026-10-26 from *AI-Powered Analytics Delivery Framework* Part 5 and *Executive Analytics Framework Playbook* Part 4 — near-identical content in both sources (same "5-Second Rule," same storytelling layers, same visual design rules), merged into one file.

## The fundamental principle

Most dashboards answer "what data exists?" Strong dashboards answer "what decision should this user make?" Design around decisions, not data availability. The dashboard is not the product — the decision is the product.

## The 5-Second Rule

Every visual should answer: *what should the user understand within 5 seconds?* If the answer is unclear, redesign the visual.

## The Dashboard Storytelling Framework (4 layers, every page)

1. **Executive Question** — e.g. "Is the Program Growing?" The user immediately knows why the page exists.
2. **Executive Answer** — stated *before* detailed analysis, e.g. "Historically yes. Recently stable."
3. **Evidence** — KPIs, charts, benchmarks, forecasts. Evidence supports the answer; it does not replace it.
4. **Executive Insight** — the business conclusion, e.g. "Most growth occurred between 2006 and 2010. Recent participation has stabilized, indicating a mature operating model."

Strong storytelling: `Question → Answer → Evidence → Conclusion → Action`. Weak storytelling: `Chart → Chart → Chart → Table`, which leaves interpretation to the user.

## Visual design principles

1. **Don't show everything.** Highlight what matters — label First Year / Peak Year / Latest Year, not every year.
2. **Use annotations.** Peak Year, Expansion Phase, Forecast Point, Highest/Lowest Month — annotations explain significance a bare chart can't.
3. **Use visual grouping.** Grouping related periods (e.g. "Expansion Phase 2006–2010" as one shaded band) communicates faster than individual values.
4. **Benchmarks create context.** "9,358" alone is just a number; "Actual 9,358, Benchmark 9,435, Variance −0.8%, On Target" is meaningful (subject to this skill's BRD-gated threshold-language caveat in `kpi-benchmark-forecast-standards.md`).
5. **Narrative before calculation.** The user understands *what happened* before *how it was calculated* — methodology belongs on a later page, not the headline.
6. **Separate strategic and operational information.** Mixing long-term lifecycle framing with monthly operational benchmarks on the same page creates confusion.

## Page architecture (default 4-page model)

| Page | Purpose | Contains |
|---|---|---|
| 1. Executive Summary | Tell the story | Executive KPIs, key findings, forecast, takeaways |
| 2. Strategic Analysis | Explain how we got here | Lifecycle analysis, historical evolution, era comparisons |
| 3. Operational Planning | Support planning decisions | Benchmarks, seasonality, operational monitoring, capacity planning |
| 4. Methodology | Build trust | Definitions, calculations, forecasting, governance |

This is the default shape for a single-dimension, time-only report (what Phase 1 actually shipped). A BRD that adds dimensional driver analysis (Centre, Region, Animal Type) needs additional page(s) for driver and intersection views — see `eda-and-business-questions.md`'s scope-drift guardrail for how those drivers must be sourced.

## Executive Insights Framework (per page)

**Observation** (what happened?) → **Interpretation** (why?) → **Impact** (why does it matter?) → **Action** (what should users do?). Example: Observation — "Foster activity peaks during June." Interpretation — "Seasonal participation patterns remain consistent." Impact — "Capacity requirements increase during summer months." Action — "Recruit foster capacity before May."

## Common dashboard mistakes

1. Starting with charts instead of the question.
2. Too many KPIs — prefer fewer, stronger ones with real business meaning.
3. No benchmarks — every KPI needs context.
4. No insights — every page needs an executive conclusion.
5. Repeating the same story across pages — each page should answer a *different* question.

## UX review checklist

Can a Director understand the page in 5 seconds? Does every visual answer a business question? Does every KPI support a decision? Are benchmarks visible? Are important points annotated? Is there a clear executive insight? Is unnecessary detail removed? Any "no" → revise the page.
