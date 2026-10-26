# Measure Display-Folder Reorganization Plan

Generated against the governance standard in `seam.md` (Domain KPI vs. UI
Formatting vs. Dynamic Title/Text, with a `_Formatting & Display` subfolder
per domain). Classification is heuristic (name-suffix rules + a DAX
return-shape check for unsuffixed text measures) — rows marked with
anything other than "high" confidence should be eyeballed before applying,
not trusted blindly.

**Nothing in `KPI.tmdl` has been changed.** This is a plan for review only.

**Scope note:** this table covers the **110 measures that will remain**
after the 31 unused measures identified in `measure-usage-audit.md` are
deleted — those 31 are excluded here since reassigning a folder for a
measure that's being removed is moot. See that file for the deletion list.

## Scope summary

| Classification | Count | Rule per seam.md |
|---|---|---|
| Domain KPI | 72 | Stays in its current domain root folder, visible |
| UI Formatting (colors) | 8 | Moves to `<Domain>\_Formatting & Display`, hidden |
| Dynamic Title/Text | 30 | Moves to `<Domain>\_Formatting & Display`, stays visible (it's the actual displayed content) |

This confirms the scope is much larger than just the color measures —
**38 of 110 measures** move under a
`_Formatting & Display` subfolder once every text-returning measure is
included, not just the ~8 color/narrative measures originally sketched.

## Full reclassification table

| Measure | Current Folder | Classification | Confidence | Proposed New Folder | Proposed IsHidden |
|---|---|---|---|---|---|
| Fostered Animals | Capacity & Support | Domain KPI | default (no color/text signal found) | Capacity & Support | false |
| Latest Reporting Date | Capacity & Support\Historical Foster Animals | Domain KPI | default (no color/text signal found) | Capacity & Support\Historical Foster Animals | false |
| Latest Reporting Year | Capacity & Support\Historical Foster Animals | Domain KPI | default (no color/text signal found) | Capacity & Support\Historical Foster Animals | false |
| Program Growth | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Historical Fostered Animals | Capacity & Support\Historical Foster Animals | Domain KPI | default (no color/text signal found) | Capacity & Support\Historical Foster Animals | false |
| First Reporting Year | Capacity & Support\Historical Foster Animals | Domain KPI | default (no color/text signal found) | Capacity & Support\Historical Foster Animals | false |
| Historical Participation Label | Capacity & Support\Historical Foster Animals | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Baseline Fostered Animals | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Latest Completed Year Fostered Animals | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Latest Completed Year | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Program Growth Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| 5-Year Monthly Benchmark | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Latest Month Fostered Animals | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Monthly Benchmark Variance % | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Monthly Performance Status | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Prior Year Same Month Fostered Animals | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Prior Year Same Month Variance % | Capacity & Support\Last Month Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\Last Month Comparison | false |
| Prior Year Same Month Comparison Label | Capacity & Support\Last Month Comparison | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Prior Year YTD Fostered Animals | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| YTD Fostered Animals | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| Prior Year YTD Variance % | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| Prior Year YTD Comparison Label | Capacity & Support\YTD Comparison | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| 5-Year YTD Benchmark | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| YTD Benchmark Variance % | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| YTD Performance Status | Capacity & Support\YTD Comparison | Domain KPI | default (no color/text signal found) | Capacity & Support\YTD Comparison | false |
| Historical Completion Ratio | Capacity & Support\Year-End Projection | Domain KPI | default (no color/text signal found) | Capacity & Support\Year-End Projection | false |
| Year-End Projection | Capacity & Support\Year-End Projection | Domain KPI | default (no color/text signal found) | Capacity & Support\Year-End Projection | false |
| Forecast Variance % | Capacity & Support\Year-End Projection | Domain KPI | default (no color/text signal found) | Capacity & Support\Year-End Projection | false |
| Forecast Status | Capacity & Support\Year-End Projection | Domain KPI | default (no color/text signal found) | Capacity & Support\Year-End Projection | false |
| Long-Term CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Long-Term CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Recent CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Recent CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Expansion Phase Color | Capacity & Support\Program Growth | UI Formatting | high | Capacity & Support\_Formatting & Display | true |
| Peak Activity Color | Capacity & Support\Program Growth | UI Formatting | high | Capacity & Support\_Formatting & Display | true |
| Stable Operations Color | Capacity & Support\Program Growth | UI Formatting | high | Capacity & Support\_Formatting & Display | true |
| Mature Program Color | Capacity & Support\Program Growth | UI Formatting | high | Capacity & Support\_Formatting & Display | true |
| Peak Fostered Animals | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Peak Year | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Completed Year Filter | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Program Status | Capacity & Support\Executive Briefing | Domain KPI | default (no color/text signal found) | Capacity & Support\Executive Briefing | false |
| Reporting Period Label | Capacity & Support\Executive Briefing | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Program Status Description | Capacity & Support\Executive Briefing | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Executive Summary Narrative | Capacity & Support\Executive Briefing | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Growth Headline | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Growth Subheadline | Capacity & Support\Program Growth | Dynamic Title/Text | medium (bare string literal, unsuffixed name) | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| 5-Year Seasonal Benchmark | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Peak Month | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Peak Month Fostered Animals | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Lowest Month | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Lowest Month Fostered Animals | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Seasonal Range % | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Peak Season Start Month | Capacity & Support\Seasonality | Dynamic Title/Text | medium (bare string literal, unsuffixed name) | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Peak Season End Month | Capacity & Support\Seasonality | Dynamic Title/Text | medium (bare string literal, unsuffixed name) | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Seasonality Headline | Capacity & Support\Seasonality | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Seasonality Description | Capacity & Support\Seasonality | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Seasonality Executive Insight | Capacity & Support\Seasonality | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Recent Stability Headline | Capacity & Support\Recent Stability | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Recent Stability Description | Capacity & Support\Recent Stability | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Recent Years Filter | Capacity & Support\Recent Stability | Domain KPI | default (no color/text signal found) | Capacity & Support\Recent Stability | false |
| Recent Average Fostered Animals | Capacity & Support\Recent Stability | Domain KPI | default (no color/text signal found) | Capacity & Support\Recent Stability | false |
| Recent Average Label | Capacity & Support\Recent Stability | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Highest Recent Year | Capacity & Support\Recent Stability | Domain KPI | default (no color/text signal found) | Capacity & Support\Recent Stability | false |
| Highest Recent Year Fostered Animals | Capacity & Support\Recent Stability | Domain KPI | default (no color/text signal found) | Capacity & Support\Recent Stability | false |
| Recent Stability Executive Insight | Capacity & Support\Recent Stability | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Forecast Outlook Headline | Forecast Outlook | Dynamic Title/Text | high | Forecast Outlook\_Formatting & Display | false (visible — user-facing text content) |
| Forecast Outlook Description | Forecast Outlook | Dynamic Title/Text | high | Forecast Outlook\_Formatting & Display | false (visible — user-facing text content) |
| Historical Forecast | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Forecast Validation Filter | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Forecast Error % | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Average Forecast Error % | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Average Forecast Error Label | Forecast Outlook | Dynamic Title/Text | high | Forecast Outlook\_Formatting & Display | false (visible — user-facing text content) |
| Planning Outlook Low | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Planning Outlook High | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Forecast Executive Insight | Forecast Outlook | Dynamic Title/Text | high | Forecast Outlook\_Formatting & Display | false (visible — user-facing text content) |
| Planning Outlook Midpoint | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Historical Actuals Series | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Projection Series | Forecast Outlook | Domain KPI | default (no color/text signal found) | Forecast Outlook | false |
| Expansion Phase Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Peak Activity Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Stable Operations Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Mature Program Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Largest Era Name | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Largest Era Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Current vs Peak Era % | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Current Era Fostered Animals | Lifecycle Analysis | Domain KPI | default (no color/text signal found) | Lifecycle Analysis | false |
| Monthly Participation | Seasonal Operations | Domain KPI | default (no color/text signal found) | Seasonal Operations | false |
| Seasonal Benchmark Variance % | Seasonal Operations | Domain KPI | default (no color/text signal found) | Seasonal Operations | false |
| Planning Priority | Seasonal Operations | Domain KPI | default (no color/text signal found) | Seasonal Operations | false |
| Monthly Foster Participation | Seasonal Operations | Domain KPI | default (no color/text signal found) | Seasonal Operations | false |
| Seasonality Heatmap Color | Seasonal Operations | UI Formatting | high | Seasonal Operations\_Formatting & Display | true |
| Seasonality Executive Conclusion | Seasonal Operations | Dynamic Title/Text | high | Seasonal Operations\_Formatting & Display | false (visible — user-facing text content) |
| Report Title | Capacity & Support\Titles | Domain KPI | default (no color/text signal found) | Capacity & Support\Titles | false |
| Expansion Phase CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Peak Activity CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Stable Operations CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Mature Program CAGR | Capacity & Support\Program Growth | Domain KPI | default (no color/text signal found) | Capacity & Support\Program Growth | false |
| Expansion Phase CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Peak Activity CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Stable Operations CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Mature Program CAGR Label | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Grown Insight | Capacity & Support\Program Growth | Dynamic Title/Text | high | Capacity & Support\_Formatting & Display | false (visible — user-facing text content) |
| Forecast Error Color | Forecast Outlook | UI Formatting | high | Forecast Outlook\_Formatting & Display | true |
| Current Year Lowest Month Fostered Animals | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Current Year Lowest Month | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Current Year Peak Month | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Current Year Peak Month Fostered Animals | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Current Year Seasonal Variation % | Capacity & Support\Seasonality | Domain KPI | default (no color/text signal found) | Capacity & Support\Seasonality | false |
| Seasonal Benchmark Variance Color | Capacity & Support\Seasonality | UI Formatting | high | Capacity & Support\_Formatting & Display | true |
| Seasonal Heatmap Font Color | Seasonal Operations | UI Formatting | high | Seasonal Operations\_Formatting & Display | true |

## Notes / caveats

- **"Domain KPI" measures with "(needs a domain assigned)"** currently have
  no `displayFolder` at all — these need an explicit domain decision before
  the reorg can be applied, they weren't silently defaulted to anything.
- **Confidence below "high"** means the classification came from inspecting
  the DAX body shape (bare string literal or `&` concatenation) rather than
  an unambiguous name suffix — worth a quick manual check per row.
- This plan does not yet implement the spec's separate **"_Base Measures"**
  category (hidden raw-aggregation helpers) — none of the current 110
  measures cleanly fit that bucket (they're all either directly consumed or
  analytically meaningful intermediates, not raw `SUM`/`COUNT` plumbing),
  so it's omitted rather than forced.
- `Fostered Animals` and similar measures used **both** directly by visuals
  **and** as a dependency by dozens of other measures stay classified as
  Domain KPI (visible) — hiding a measure that's directly bound to a visual
  would break that visual's field reference.
