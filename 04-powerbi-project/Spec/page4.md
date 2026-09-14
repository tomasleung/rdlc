# Page 4 Redesign Specification
## Definitions, Methodology & Trust Center

### Purpose

This page explains:

```text
What does the dashboard measure?

How are benchmarks calculated?

How is the forecast calculated?

Can the forecast be trusted?
```

Audience:

```text
Program Owners

Directors

Analysts
```

The page should use business language rather than technical implementation details.

Avoid:

```text
DAX

Fact Tables

Lineage Tags

Power BI Technical Details
```

Focus on:

```text
Definitions

Methodology

Transparency

Trust
```

---

# Page Layout

Page 4 contains four sections:

```text
Primary Reporting Metric

Benchmark Methodology

Forecast Methodology

Forecast Validation
```

followed by an Executive Insight.

---

# Section 1 - Primary Reporting Metric

## Title

```text
Primary Reporting Metric
```

---

## Foster Animal Count

### Business Definition

```text
Count of distinct animals that were in foster
at any point during the reporting period
(month or year).

An animal placed in foster more than once
during the same reporting period is counted once.
```

---

### Reporting Logic

```text
Distinct animal, per period
(month or year touched).
```

---

### Why We Use This Metric

```text
Foster Animal Count measures program reach
and participation.

It represents the number of unique animals
receiving foster support and is used throughout
the dashboard for:

- Growth Analysis
- Seasonality Analysis
- Benchmarking
- Forecasting
- Executive Reporting
```

---

## Visual Design

Display as a business glossary card.

Example:

```text
Metric

Foster Animal Count

Definition

Distinct animals receiving foster support
during the selected reporting period.

Purpose

Measures program reach and participation.
```

---

# Section 2 - Benchmark Methodology

## Title

```text
How Benchmarks Are Calculated
```

---

## Monthly Benchmark Method

```text
Monthly benchmarks are calculated using
the average foster participation from the
five most recent complete reporting years
(2021-2025).
```

Purpose:

```text
Represent expected activity levels during
the current mature operating period.
```

---

## Formula

```text
Benchmark

=
Average(
2021,
2022,
2023,
2024,
2025
)
```

---

## Example

### August Benchmark

```text
2021 = 9,285

2022 = 9,473

2023 = 9,636

2024 = 9,417

2025 = 9,362
```

Benchmark:

```text
9,435
```

---

## Performance Status Rules

| Status | Variance From Benchmark |
|----------|----------|
| 🟢 On Target | Within ±10% |
| 🟡 Monitor | Between 10% and 15% |
| 🔴 Action Required | Greater than 15% |

---

## Visual Design

Use a simple methodology card with:

```text
5-Year Historical Average

2021-2025

Used for:
Monthly Benchmarks
YTD Benchmarks
Operational Planning
```

---

# Section 3 - Forecast Methodology

## Title

```text
How The Forecast Is Calculated
```

---

## Method

```text
Historical Ratio Forecasting
```

---

## Business Explanation

```text
Historical analysis shows that approximately
88.5% of annual foster participation has
typically occurred by the end of August.
```

This pattern is used to estimate
full-year participation.

---

## Historical Completion Rates

```text
2021 = 87.7%

2022 = 87.9%

2023 = 88.0%

2024 = 90.1%

2025 = 89.0%
```

Average:

```text
88.5%
```

---

## 2026 Projection

### Actual YTD

```text
11,312
```

### Calculation

```text
11,312
÷
88.5%
```

### Projection

```text
12,800
```

---

## Business Interpretation

```text
The forecast uses historical participation
completion patterns rather than complex
statistical forecasting models.

This approach provides a transparent and
easy-to-understand projection method.
```

---

## Visual Design

Use a simple step-by-step flow:

```text
August YTD

11,312

↓

Historical Completion Rate

88.5%

↓

Projected Year-End

12,800
```

---

# Section 4 - Forecast Validation

## Title

```text
How We Validate The Forecast
```

---

## Purpose

```text
Forecast accuracy is tested using completed
historical years to determine how well the
method would have performed.
```

---

## Historical Validation Results

| Year | Forecast | Actual | Error |
|------|---------:|---------:|---------:|
| 2021 | 13,074 | 13,197 | -0.9% |
| 2022 | 13,827 | 13,915 | -0.6% |
| 2023 | 13,770 | 13,843 | -0.5% |
| 2024 | 13,371 | 13,135 | +1.8% |
| 2025 | 12,850 | 12,778 | +0.6% |

---

## Validation KPI

### Historical Forecast Error

```text
±0.9%
```

---

## Business Interpretation

```text
Historical testing shows that the forecasting
method typically performs within approximately
1% of actual year-end results.

This provides confidence that the 2026 projection
is consistent with historical participation patterns.
```

---

## Visual Design

Display:

```text
Historical Forecast Error

±0.9%
```

as the primary KPI on the page.

The validation table should be the largest visual
element in this section.

---

# Executive Insight

## Title

```text
Why You Can Trust This Dashboard
```

---

## Executive Insight Text

```text
The dashboard uses Foster Animal Count as its
primary reporting metric because it represents
the number of unique animals receiving foster care.

Monthly benchmarks are based on recent historical
participation patterns (2021-2025), while year-end
projections use Historical Ratio Forecasting based
on historical August completion rates.

Historical validation produced an average forecast
error of approximately ±0.9%, demonstrating that
the forecasting methodology performs well against
completed reporting years.

Together, these methods provide a consistent,
transparent, and evidence-based foundation for
monitoring performance and planning future foster
program operations.
```

---

# What Users Should Understand In 5 Seconds

```text
Foster Animal Count is the primary metric.

Benchmarks are based on 2021-2025 averages.

2026 forecast uses historical completion rates.

Historical forecast error is only ±0.9%.

The methodology is transparent and validated.
```

---

# Design Principles

✅ Business language

✅ Simple explanations

✅ Transparent calculations

✅ Forecast validation visible

✅ Trust-building content

✅ Minimal technical jargon

❌ No DAX formulas

❌ No semantic model diagrams

❌ No table lineage

❌ No Fabric architecture content

❌ No developer-focused details

---

What does the metric mean?
 
How are benchmarks calculated?
 
Can I trust the forecast?


Based on historical August completion rates:

2021 = 87.7% = YTD august / Full
2022 = 87.9%
2023 = 88.0%
2024 = 90.1%
2025 = 89.0%

Average = 88.5%