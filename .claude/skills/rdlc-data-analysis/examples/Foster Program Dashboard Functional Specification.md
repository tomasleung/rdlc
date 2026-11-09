# Foster Program Dashboard Functional Specification
## Part 1 - Project Overview and Solution Architecture

### Purpose

This document serves as the functional specification for the Foster Program Executive Dashboard.

The document describes:

- Business objectives
- Reporting requirements
- Target audience
- Data model
- KPI definitions
- Visual specifications
- Benchmark methodology
- Forecast methodology
- Governance standards

The objective is to provide sufficient documentation for:

- Power BI Developers
- Microsoft Fabric Developers
- Data Analysts
- Program Owners
- Future AI Agents

to maintain and enhance the solution.

---

# Business Objective

The Foster Program Dashboard is designed to support decision-making for:

### Primary Audience

Program Owners

Responsibilities:

```text
Operational Planning

Capacity Planning

Volunteer Recruitment

Resource Allocation

Performance Monitoring
```

---

### Secondary Audience

Directors

Responsibilities:

```text
Strategic Planning

Program Oversight

Performance Monitoring

Forecast Review
```

---

# Dashboard Mission

Provide a decision-support system that answers:

```text
Is the Program Growing?

Is There Seasonality?

What Is Happening Recently?

What Should We Expect Next?
```

while supporting future operational planning.

---

# Solution Architecture

## Reporting Platform

```text
Microsoft Fabric
```

---

## Reporting Tool

```text
Power BI
```

---

## Modeling Approach

```text
Kimball Style Star Schema
```

with:

```text
Fact Tables

Dimension Tables

Reusable Measures
```

---

# Data Model Overview

## Fact Table

### Foster Placements

Business Grain

```text
One row = One Foster Placement
```

Purpose

```text
Operational Analysis

Placement Analysis

Duration Analysis

Current Foster Inventory
```

Key Columns

```text
Foster Placement ID

SB Animal ID

Foster Start Date Key

Foster End Date Key

Currently In Foster

Foster Duration (Days)

Foster Location
```

---

## Reporting Snapshot Table

### Foster Animal Month

Business Grain

```text
One row = One Animal During One Calendar Month
```

Purpose

```text
Monthly Reporting

Trend Analysis

Seasonality Analysis

Benchmarking

Forecasting

Executive Reporting
```

Key Columns

```text
SB Animal ID

Month Date Key

Foster Placement ID
```

---

## Dimension Table

### Date

Purpose

```text
Time Intelligence

Trend Reporting

Benchmarking

Forecasting
```

Attributes

```text
Date

Date Key

Year

Quarter

Month

Month Name

Month Short

Season

Week
```

---

# Relationship Model

```text
Date
    ↓
Foster Animal Month

Date
    ↓
Foster Placements
```

Relationship Type

```text
One-to-Many
```

Date acts as the conformed reporting dimension.

---

# Semantic Model Standards

## Reporting Standard

Primary Metric

```text
Fostered Animals
```

Purpose

```text
Measure program reach and participation.
```

Reason

```text
Represents unique animals receiving foster support.

Supports:
- Trend Analysis
- Growth Analysis
- Benchmarking
- Seasonality Analysis
- Forecasting
```

This is the primary measure used throughout the dashboard.

---

## Secondary Metrics

Used for supplemental operational reporting.

### Foster Placement Count

```text
Counts placement events.
```

Use Cases

```text
Capacity Analysis

Operational Volume
```

---

### Current Foster Animals

```text
Animals currently in foster care.
```

Use Cases

```text
Operational Inventory

Capacity Monitoring
```

---

### Average Foster Duration

```text
Average days spent in foster care.
```

Use Cases

```text
Operational Efficiency

Program Effectiveness
```

---

# Dashboard Design Principles

The report must follow:

```text
Question
↓
Answer
↓
Evidence
↓
Insight
↓
Decision
```

Every page must contain:

### Business Question

### Executive Answer

### Supporting Visuals

### Supporting KPIs

### Executive Insight

---

# Dashboard Structure

## Page 1

Executive Summary

Purpose

```text
Tell leadership what matters most.
```

Questions

```text
Q1 Is the Program Growing?

Q2 Is There Seasonality?

Q3 What Is Happening Recently?

Q4 What Should We Expect Next?
```

---

## Page 2

Program Lifecycle Analysis

Purpose

```text
Explain how the program evolved.
```

Questions

```text
How has the Foster Program evolved?
```

---

## Page 3

Capacity Planning & Seasonal Operations

Purpose

```text
Support operational planning.
```

Questions

```text
How should Program Owners prepare for seasonal demand?
```

---

## Page 4

Methodology & Governance

Purpose

```text
Explain calculations.

Build trust.

Support reproducibility.
```

---

# Success Criteria

The dashboard succeeds when users can answer:

### Directors

```text
How is the program performing?

Is growth continuing?

What should we expect next?
```

---

### Program Owners

```text
When should capacity increase?

How are we performing versus benchmark?

What seasonal patterns require planning?
```

---

# Developer Design Rule

Every visual must answer:

```text
What should the user understand within 5 seconds?
```

If that answer is unclear:

```text
Redesign the visual.
```

---

# Part 1 Summary

The Foster Program Dashboard is designed as a decision-support system built on:

```text
Foster Animal Month
↓
Fostered Animals
↓
Benchmarks
↓
Forecasting
↓
Executive Storytelling
```

The dashboard prioritizes business understanding over visual complexity and uses a question-driven design framework to support both strategic and operational decision-making.

# End Of Part 1

Next Block:

Part 2 - KPI Catalog, KPI Specifications, Executive Summary Page (Page 1), and Executive KPI Framework
# Foster Program Dashboard Functional Specification
## Part 2 - KPI Catalog, KPI Specifications, and Executive Summary Page (Page 1)

### Purpose

Page 1 serves as the Executive Summary.

The objective is to enable Directors and Program Owners to understand:

```text
Program Size

Historical Growth

Recent Performance

Current Year Performance

Expected Year-End Outcome
```

within a few seconds.

---

# Page 1 Architecture

Executive Summary is organized around four business questions.

---

## Q1

```text
Is the Program Growing?
```

Executive Answer:

```text
Historically yes.

Recently stable.
```

---

## Q2

```text
Is There Seasonality?
```

Executive Answer:

```text
Yes.

Peak activity consistently occurs between May and August.
```

---

## Q3

```text
What Is Happening Recently?
```

Executive Answer:

```text
Activity remains within a mature operating range.
```

---

## Q4

```text
What Should We Expect Next?
```

Executive Answer:

```text
2026 is projected to finish near 2025 levels.
```

---

# Executive KPI Framework

The KPI row provides the executive story before users review detailed visuals.

The sequence is intentional:

```text
Program Size
↓
Program Growth
↓
Recent Month Performance
↓
Year-To-Date Performance
↓
Expected Outcome
```

---

# KPI #1

## Historical Foster Animals

Value:

```text
109,738
```

Subtext:

```text
2006–2025 Foster Participation
```

Business Question:

```text
How large is the Foster Program?
```

Measure:

```text
Fostered Animals
```

Business Meaning:

```text
Represents the total number of distinct animals
that received foster support across all complete
reporting years.
```

---

# KPI #2

## Program Growth Since 2006

Value:

```text
+144.6%
```

Subtext:

```text
2006: 5,223 → 2025: 12,778
```

Business Question:

```text
How much has the program grown?
```

Calculation:

```text
(12,778 - 5,223)
÷
5,223

=
144.6%
```

Business Meaning:

```text
Measures growth relative to the first complete
reporting year.
```

---

# KPI #3

## August 2026 Performance

Value:

```text
9,358
```

Subtext:

```text
▼ 0.04% vs August 2025
```

Benchmark:

```text
5-Year Average Benchmark

9,435
```

Variance:

```text
-0.8%
```

Status:

```text
🟢 On Target
```

Business Question:

```text
Did the most recent month meet expectations?
```

Calculation:

```text
Actual

=
9,358
```

```text
Benchmark

=
9,435
```

```text
Variance

=
(9,358 - 9,435)
÷
9,435

=
-0.8%
```

---

# KPI #4

## 2026 YTD Fostered Animals

Value:

```text
11,312
```

Subtext:

```text
Through Aug 2026
```

Comparison:

```text
▼ 0.5% vs Aug 2025 YTD
```

Benchmark:

```text
5-Year Average YTD Benchmark

11,840
```

Variance:

```text
-4.5%
```

Status:

```text
🟡 Slightly Below Benchmark
```

Business Question:

```text
How is the year performing so far?
```

Business Meaning:

```text
Measures cumulative participation compared to
historical expectations.
```

---

# KPI #5

## 2026 Year-End Projection

Value:

```text
12,800
```

Subtext:

```text
2025 Actual: 12,778
```

Additional Context:

```text
Historical Forecast Error

±0.9%
```

Status:

```text
🟢 Projected to Finish Near 2025 Levels
```

Business Question:

```text
What should we expect next?
```

Forecast Method:

```text
Historical Ratio Forecasting
```

Business Meaning:

```text
Projects full-year foster participation using
historical completion rates.
```

---

# Executive Briefing

Purpose:

Provide the executive narrative before users review visuals.

---

## Executive Briefing Text

```text
The Foster Program has supported more than
109,000 foster animals since 2006 and experienced
significant growth during its early expansion period.

Analysis indicates that participation has stabilized
during recent years, suggesting the program has
transitioned into a mature operating model.

Seasonal participation patterns remain highly
predictable, with activity consistently peaking
between May and August.

Historical Ratio Forecasting projects approximately
12,800 fostered animals in 2026, which is nearly
identical to 2025 performance and supports the
conclusion that the Foster Program remains stable.
```

---

# Program Status Panel

Display:

```text
Mature & Stable
```

Supporting Text:

```text
Current participation remains consistent with
recent historical performance and forecasted
activity remains within expected operating levels.
```

---

# Executive KPI Design Rules

All KPI cards should:

✅ Answer a business question

✅ Contain context

✅ Include benchmark references where applicable

✅ Support decision-making

✅ Be understandable in under 5 seconds

---

# KPI Design Pattern

Every KPI on Page 1 should follow:

```text
Title
↓
Value
↓
Context
↓
Benchmark
↓
Status
↓
Business Meaning
```

This structure creates consistency and improves executive readability.

---

# Page 1 Success Criteria

A Director viewing only the KPI row should understand:

```text
Program supported 109K+ animals.

Historical growth was significant.

August performed near expectation.

2026 is slightly below benchmark YTD.

Year-end participation is projected
to finish near 2025 levels.

Program remains mature and stable.
```

---

# Part 2 Summary

Page 1 establishes the executive narrative using:

```text
Historical Scale
↓
Historical Growth
↓
Recent Performance
↓
Current Year Performance
↓
Future Outlook
```

and serves as the foundation for all subsequent analytical pages.

# End Of Part 2

Next Block:

Part 3 - Page 1 Visual Specifications (Q1, Q2, Q3, Q4) and Executive Takeaways
# Foster Program Dashboard Functional Specification
## Part 3 - Page 1 Visual Specifications and Executive Summary Design

### Purpose

Page 1 serves as the Executive Summary page and must provide leadership with a complete understanding of:

```text
Historical Performance

Current Performance

Seasonality

Future Outlook
```

within a single page.

The page should answer all major executive questions before users navigate to supporting pages.

---

# Page Layout

Page 1 contains:

```text
Executive Briefing

Program Status

5 Executive KPIs

Q1 Growth Analysis

Q2 Seasonality Analysis

Q3 Recent Performance

Q4 Forecast Analysis

Executive Conclusion
```

---

# Q1 - Is The Program Growing?

## Executive Answer

```text
Historically yes.

Recently stable.
```

---

## Visual Type

Line Chart

---

## Data

Annual Fostered Animals

```text
2006  5,223
2007  8,444
2008 11,689
2009 15,246
2010 15,763
2011 15,709
2012 15,047
2013 14,127
2014 14,420
2015 14,207
2016 14,656
2017 14,341
2018 13,781
2019 13,911
2020 13,510
2021 13,197
2022 13,915
2023 13,843
2024 13,135
2025 12,778
```

---

## Visual Design

### Expansion Phase

```text
2006-2010

CAGR +31.8%
```

Use:

```text
Light Blue Background
```

---

### Mature Operating Phase

```text
2011-2025

CAGR -1.4%
```

Use:

```text
Light Green Background
```

---

## Chart Callouts

### First Reporting Year

```text
2006

5,223
```

---

### Peak Year

```text
2010

15,763
```

---

### Latest Complete Year

```text
2025

12,778
```

---

## Supporting Metrics

### Long-Term CAGR

```text
+4.8%
```

Definition:

```text
2006-2025 CAGR
```

---

### Recent CAGR

```text
-0.8%
```

Definition:

```text
2021-2025 CAGR
```

---

## Executive Insight

```text
The Foster Program experienced rapid expansion
between 2006 and 2010 before transitioning into
a mature operating model.

Recent participation has remained relatively stable,
indicating that significant growth has largely
plateaued while program participation remains strong.
```

---

# Q2 - Is There Seasonality?

## Executive Answer

```text
Yes.

Peak activity consistently occurs between May and August.
```

---

## Visual Type

Area Chart

---

## Data Source

5-Year Monthly Benchmark Average

```text
Jan  9,137
Feb  9,046
Mar  9,114
Apr  9,232
May  9,369
Jun  9,511
Jul  9,484
Aug  9,435
Sep  9,439
Oct  9,446
Nov  9,321
Dec  9,234
```

---

## Visual Design

Use:

```text
Orange Area Chart
```

with:

```text
May-August Highlight Band
```

---

## Callouts

### Lowest Activity

```text
February

9,046
```

---

### Peak Activity

```text
June

9,511
```

---

### Peak Activity Window

```text
May-August
```

---

## Supporting Metrics

### Peak Month

```text
June

9,511
```

---

### Lowest Month

```text
February

9,046
```

---

### Seasonal Variation

```text
+5.1%
```

Calculation:

```text
(9,511 - 9,046)
÷
9,046
```

---

## Executive Insight

```text
Foster participation follows a highly predictable
seasonal pattern.

Participation rises through spring, peaks between
May and August, and remains elevated through early
fall before moderating during the winter months.
```

---

# Q3 - What Is Happening Recently?

## Executive Answer

```text
Recent participation remains stable within a
mature operating range.
```

---

## Visual Type

Column Chart

---

## Data

```text
2021 13,197
2022 13,915
2023 13,843
2024 13,135
2025 12,778
2026 11,312 YTD
```

---

## Design

### 2021-2025

Use:

```text
Green Columns
```

---

### 2026

Use:

```text
Light Green YTD Column
```

Label:

```text
YTD Through August
```

---

## Reference Line

```text
5-Year Average

13,374
```

Label:

```text
Recent Operating Range
```

---

## Supporting Metrics

### Recent Average

```text
13,374
```

---

### Strongest Recent Year

```text
2022

13,915
```

---

### Recent CAGR

```text
-0.8%
```

---

## Executive Insight

```text
Participation has remained relatively stable
between 2021 and 2025.

The Foster Program is currently operating within
a mature and predictable participation range
rather than experiencing rapid growth.
```

---

# Q4 - What Should We Expect Next?

## Executive Answer

```text
2026 is projected to finish near 2025 levels.
```

---

## Visual Type

Forecast Line Chart

---

## Historical Actuals

```text
2021 13,197
2022 13,915
2023 13,843
2024 13,135
2025 12,778
```

---

## Forecast

```text
2026 12,800
```

---

## Outlook

```text
2027 Planning Outlook

12,700-12,900
```

Display midpoint:

```text
12,800
```

Label:

```text
Planning Outlook
```

---

## Forecast Validation Table

| Year | Forecast | Actual | Error |
|--------|--------:|--------:|--------:|
| 2021 | 13,074 | 13,197 | -0.9% |
| 2022 | 13,827 | 13,915 | -0.6% |
| 2023 | 13,770 | 13,843 | -0.5% |
| 2024 | 13,371 | 13,135 | +1.8% |
| 2025 | 12,850 | 12,778 | +0.6% |

---

## Supporting Metrics

### 2025 Actual

```text
12,778
```

---

### 2026 Projection

```text
12,800
```

---

### Historical Forecast Error

```text
±0.9%
```

---

## Executive Insight

```text
Historical Ratio Forecasting projects approximately
12,800 fostered animals in 2026.

Historical validation produced an average forecast
error of approximately ±0.9%, indicating that the
forecast method performs well relative to historical
results.

The forecast suggests participation will remain
consistent with 2025 activity levels.
```

---

# Executive Conclusion Section

## Title

```text
Key Findings & Executive Conclusion
```

---

## Summary Metrics

### Historical Foster Animals

```text
109,738
```

---

### Program Growth

```text
+144.6%
```

---

### Peak Activity Window

```text
May-August
```

---

### Recent Operating Range

```text
13K-14K
```

---

### 2026 Projection

```text
12,800

Forecast Error ±0.9%
```

---

## Executive Conclusion

```text
The Foster Program has transitioned from a period
of rapid growth into a mature and stable operating
model.

Most growth occurred between 2006 and 2010.
Participation now follows a highly predictable
seasonal pattern, with peak activity occurring
between May and August.

Historical Ratio Forecasting suggests 2026 will
finish near 2025 participation levels, reinforcing
the conclusion that the program remains stable,
predictable, and operationally sustainable.
```

# End Of Part 3

Next Block:

Part 4 - Page 2 Program Lifecycle Analysis and Strategic Evolution
# Foster Program Dashboard Functional Specification
## Part 4 - Program Lifecycle Analysis (Page 2)

### Purpose

Page 2 provides strategic context for Program Owners and Directors.

Page 1 answers:

```text
What is happening?
```

Page 2 answers:

```text
How did we get here?
```

The objective is to explain the evolution of the Foster Program over time and demonstrate how the program transitioned from rapid expansion into a mature operating model.

---

# Business Question

```text
How has the Foster Program evolved over time?
```

---

# Executive Answer

```text
The Foster Program evolved through four distinct lifecycle stages:

Expansion
↓
Peak Activity
↓
Stable Operations
↓
Mature Program
```

The current level of participation remains close to historical peak levels despite the transition into a mature operating phase.

---

# Page Layout

Page 2 contains four sections:

```text
Program Lifecycle Journey

Era Comparison Analysis

Lifecycle KPI Summary

Executive Insight
```

---

# Section 1 - Program Lifecycle Journey

## Purpose

Provide a strategic view of the Foster Program's historical evolution.

---

## Visual Type

Lifecycle Timeline

---

## Lifecycle Stages

### Expansion Phase

```text
2006-2009
```

Participation:

```text
24,209 Foster Animals
```

Characteristics:

```text
Rapid growth

Program expansion

Participation acceleration
```

---

### Peak Activity Phase

```text
2010-2014
```

Participation:

```text
38,844 Foster Animals
```

Characteristics:

```text
Highest participation period

Program operating at peak historical levels
```

---

### Stable Operations Phase

```text
2015-2019
```

Participation:

```text
35,409 Foster Animals
```

Characteristics:

```text
Participation stabilization

Sustained operating capacity
```

---

### Mature Program Phase

```text
2020-2025
```

Participation:

```text
36,146 Foster Animals
```

Characteristics:

```text
Stable participation

Predictable operating range

Mature operating model
```

---

## Visual Design

Use:

```text
Expansion
→
Peak
→
Stable
→
Mature
```

with directional arrows connecting each phase.

Purpose:

```text
Emphasize evolution rather than isolated periods.
```

---

# Section 2 - Era Comparison Analysis

## Purpose

Compare participation across lifecycle stages.

---

## Visual Type

Horizontal Bar Chart

---

## Data

### Expansion Phase

```text
24,209
```

---

### Peak Activity Phase

```text
38,844
```

---

### Stable Operations Phase

```text
35,409
```

---

### Mature Program Phase

```text
36,146
```

---

## Visual Rules

Maintain chronological ordering.

Do NOT sort by value.

Order:

```text
Expansion

Peak Activity

Stable Operations

Mature Program
```

The objective is to reinforce the lifecycle story.

---

## Highlighting

### Peak Activity Phase

```text
38,844
```

Use strongest visual emphasis.

Reason:

```text
Highest participation era.
```

---

### Mature Program Phase

```text
36,146
```

Use secondary emphasis.

Reason:

```text
Represents current operating state.
```

---

# Section 3 - Lifecycle KPI Summary

## KPI #1

### Largest Era

```text
Peak Activity Phase

38,844
```

Business Meaning:

```text
Highest participation period in program history.
```

---

## KPI #2

### Current Era

```text
Mature Program Phase

36,146
```

Business Meaning:

```text
Represents current operating environment.
```

---

## KPI #3

### Difference From Peak

```text
-6.9%
```

Calculation:

```text
(36,146 - 38,844)
÷
38,844

=
-6.9%
```

Business Meaning:

```text
Current participation remains very close to
historical peak-era performance.
```

Interpretation:

```text
Mature

Not Declining
```

---

# Executive Insight

Replace all previous lifecycle commentary with the following approved version.

---

## Executive Insight Text

```text
The Foster Program evolved through four distinct lifecycle stages: Expansion (2006–2009), Peak Activity (2010–2014), Stable Operations (2015–2019), and the current Mature Program Phase (2020–2025).

The Peak Activity Phase supported 38,844 foster animals, representing the highest participation period in program history. The current Mature Program Phase supported 36,146 foster animals, only 6.9% below the peak era.

Although participation has moderated from historical highs, foster activity has remained stable throughout the last decade. This pattern suggests the Foster Program has successfully transitioned from rapid growth into a mature, sustainable operating model that continues to operate near historical peak participation levels.
```

---

# Page Design Rules

✅ Focus on lifecycle evolution

✅ Emphasize historical context

✅ Use strategic language

✅ Highlight transition from growth to maturity

✅ Reinforce stability narrative

✅ Align insights to KPI values

---

# Avoid

❌ Repeating annual trend charts from Page 1

❌ Repeating CAGR analysis

❌ Repeating seasonality analysis

❌ Large data tables

❌ Operational detail

---

# Page 2 Success Criteria

A Program Owner or Director should understand within 5 seconds:

```text
The program expanded rapidly.

The program reached a participation peak.

Participation stabilized.

Current activity remains close to historical highs.

The Foster Program is now mature and sustainable.
```

---

# Part 4 Summary

Page 2 explains the strategic evolution of the Foster Program:

```text
Expansion
↓
Peak Activity
↓
Stable Operations
↓
Mature Program
```

and demonstrates that current participation levels remain close to historical peak performance despite the transition into a mature operating model.

# End Of Part 4

Next Block:

Part 5 - Capacity Planning & Seasonal Operations (Page 3)
# Foster Program Dashboard Functional Specification
## Part 5 - Capacity Planning & Seasonal Operations (Page 3)

### Purpose

Page 3 is the primary operational planning page.

Unlike Page 1, which focuses on:

```text
Executive Understanding
```

Page 3 focuses on:

```text
Operational Readiness

Capacity Planning

Benchmark Monitoring

Seasonal Preparation
```

The purpose is to help Program Owners anticipate demand rather than react to demand.

---

# Business Question

```text
How should Program Owners prepare for seasonal foster demand?
```

---

# Executive Answer

```text
Foster participation follows a highly predictable seasonal cycle.

Activity increases through spring, peaks between May and August, and remains elevated through early fall before moderating during winter months.

Historical patterns provide a reliable foundation for operational planning and capacity preparation.
```

---

# Page Layout

Page 3 contains:

```text
Seasonal Activity Pattern

Capacity Planning Benchmarks

Historical Seasonality Heatmap

Operational Planning Summary

Executive Insight
```

---

# Section 1 - Seasonal Activity Pattern

## Purpose

Compare:

```text
Expected Seasonal Pattern

vs

Current-Year Performance
```

---

## Visual Type

Dual-Line Area Chart

---

## Line 1

### 5-Year Benchmark Average (2021-2025)

Data:

```text
Jan  9,137
Feb  9,046
Mar  9,114
Apr  9,232
May  9,369
Jun  9,511
Jul  9,484
Aug  9,435
Sep  9,439
Oct  9,446
Nov  9,321
Dec  9,234
```

Style:

```text
Orange Line

Orange Area Fill
```

Purpose:

```text
Represents expected seasonal activity.
```

---

## Line 2

### 2026 Actual Monthly Performance

Data:

```text
Jan  9,032
Feb  8,887
Mar  8,932
Apr  9,069
May  9,219
Jun  9,299
Jul  9,407
Aug  9,358
```

Style:

```text
Green Line

Marker Points

No Area Fill
```

Purpose:

```text
Represents current-year performance.
```

---

## Peak Season Highlight

Highlight:

```text
May-August
```

Visual:

```text
Light Orange Shaded Band
```

Label:

```text
Peak Foster Activity Window
```

---

## Chart Callouts

### Lowest Activity

```text
February

Benchmark: 9,046
```

---

### Peak Activity

```text
June

Benchmark: 9,511
```

---

### August Position

```text
August

Actual: 9,358

Benchmark: 9,435

Variance: -0.8%
```

---

# Section 2 - Capacity Planning Benchmarks

## Purpose

Provide operational planning targets.

---

## Visual Type

Operational Benchmark Table

| Metric | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec |
|----------|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|--------:|
| 5-Year Benchmark | 9,137 | 9,046 | 9,114 | 9,232 | 9,369 | 9,511 | 9,484 | 9,435 | 9,439 | 9,446 | 9,321 | 9,234 |
| 2026 Actual | 9,032 | 8,887 | 8,932 | 9,069 | 9,219 | 9,299 | 9,407 | 9,358 | -- | -- | -- | -- |
| Variance % | -1.1% | -1.8% | -2.0% | -1.8% | -1.6% | -2.2% | -0.8% | -0.8% | -- | -- | -- | -- |
| Planning Priority | Low | Low | Moderate | Moderate | High | High | High | High | Elevated | Elevated | Moderate | Moderate |

---

## Purpose

Allow Program Owners to evaluate:

```text
Expected Performance

vs

Actual Performance

vs

Operational Priority
```

for each month.

---

# Section 3 - Historical Seasonality Heatmap

## Purpose

Provide a long-term view of participation patterns.

Reveal:

```text
Program Growth

Seasonality

Participation Stability
```

using all available historical years.

---

## Visual Type

Year-Month Heatmap

---

## Rows

Display all years:

```text
2006
2007
2008
2009
2010
2011
2012
2013
2014
2015
2016
2017
2018
2019
2020
2021
2022
2023
2024
2025
2026
```

---

## Columns

```text
Jan
Feb
Mar
Apr
May
Jun
Jul
Aug
Sep
Oct
Nov
Dec
```

---

## Design Rules

### Use Actual Values

Do NOT use:

```text
2011-2015 Average

2016-2020 Average

2021-2025 Average
```

Display all individual years.

---

### 2026

Show:

```text
Jan-Aug Actual Values
```

Display:

```text
Sep-Dec

No Data Yet
```

using neutral coloring.

---

## Visual Purpose

The heatmap should reveal:

```text
Historical Growth
↓
Program Stabilization
↓
Consistent Seasonal Peaks
↓
Mature Operating Pattern
```

within a single visual.

---

# Section 4 - Operational Planning Summary

## KPI #1

### Peak Month

```text
June

9,511
```

Business Meaning:

```text
Highest average participation month.
```

---

## KPI #2

### Lowest Month

```text
February

9,046
```

Business Meaning:

```text
Lowest average participation month.
```

---

## KPI #3

### Peak Activity Window

```text
May-August
```

Business Meaning:

```text
Primary planning period.
```

---

## KPI #4

### Seasonal Variation

```text
+5.1%
```

Calculation:

```text
(9,511 - 9,046)
÷
9,046

=
5.1%
```

Business Meaning:

```text
Difference between highest and lowest benchmark month.
```

---

# Executive Insight

```text
Foster participation follows a highly predictable seasonal pattern across all reporting years.

2026 participation has generally tracked close to the five-year benchmark, reinforcing the stability of the program's seasonal operating cycle.

Peak foster participation consistently occurs between May and August, providing Program Owners with a clear planning window for volunteer recruitment, foster engagement, and capacity readiness.

Historical benchmarking identifies June as the highest-demand month and February as the lowest-demand month, supporting proactive operational planning ahead of seasonal peaks.
```

---

# Page Design Rules

✅ Focus on planning

✅ Compare Actual vs Benchmark

✅ Use all historical years

✅ Make the heatmap the hero operational visual

✅ Highlight May-August seasonal peak

✅ Support capacity planning decisions

---

# Avoid

❌ Repeating lifecycle analysis

❌ Repeating growth analysis

❌ August-only reporting

❌ Executive-only narratives

❌ Averaging heatmap years

---

# Page 3 Success Criteria

A Program Owner should understand within 5 seconds:

```text
Seasonality is predictable.

2026 is tracking close to benchmark.

Peak demand occurs May-August.

June is the highest-demand month.

Capacity planning should occur before May.

Historical participation patterns have remained remarkably consistent across 20 years.
```

---

# Part 5 Summary

Page 3 transforms historical participation data into operational planning guidance by combining:

```text
Expected Seasonal Pattern
↓
Current-Year Performance
↓
Monthly Benchmark Monitoring
↓
Historical Pattern Recognition
↓
Capacity Planning Decisions
```

and serves as the primary planning page for Program Owners.

# End Of Part 5

Next Block:

Part 6 - Methodology & Governance (Page 4), Benchmark Methodology, Forecast Methodology, KPI Catalog, and Developer Build Checklist
# Foster Program Dashboard Functional Specification
## Part 6 - Methodology & Governance (Page 4), KPI Catalog, Forecasting Standards, and Developer Build Checklist

### Purpose

Page 4 exists to build trust.

Pages 1-3 answer business questions.

Page 4 answers:

```text
How was everything calculated?

Where did the numbers come from?

Can another analyst reproduce the results?
```

The page is intended for:

- Program Owners
- Directors
- Analysts
- Auditors
- Developers
- Future Report Maintainers

---

# Page Layout

Page 4 contains:

```text
Metric Definitions

Benchmark Methodology

Forecasting Methodology

Forecast Validation

Governance Standards

Developer Reference
```

---

# Section 1 - Metric Definitions

## Primary Reporting Metric

### Fostered Animals

Definition:

```text
Distinct animals that experienced foster care
during the selected reporting period.
```

Measure:

```DAX
Fostered Animals =
DISTINCTCOUNT(
    'Foster Animal Month'[SB Animal ID]
)
```

Business Purpose:

```text
Measures unique foster participation.

Used for:
- Trends
- Growth Analysis
- Seasonality
- Benchmarking
- Forecasting
```

---

## Supporting Operational Metrics

### Foster Placement Count

Definition:

```text
Counts each foster placement event.

Animals placed multiple times contribute
multiple placements.
```

Measure:

```DAX
COUNTROWS('Foster Placements')
```

Use Cases:

```text
Operational Volume

Placement Activity

Capacity Utilization
```

---

### Current Foster Animals

Definition:

```text
Distinct animals currently in foster care.
```

Use Cases:

```text
Current Capacity

Inventory Monitoring
```

---

### Average Foster Duration

Definition:

```text
Average number of days spent in foster care.
```

Use Cases:

```text
Efficiency

Placement Management

Program Evaluation
```

---

# Section 2 - Benchmark Methodology

## Purpose

Benchmarks establish expected performance.

They allow users to determine:

```text
Above Expectations

At Expectations

Below Expectations
```

---

## Benchmark Method

### 5-Year Historical Average

Benchmark Period:

```text
2021-2025
```

Reason:

```text
Represents the current mature operating period.
```

---

## Monthly Benchmark Formula

```text
Benchmark

=
Average(
2021 Value,
2022 Value,
2023 Value,
2024 Value,
2025 Value
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

Calculation:

```text
=
9,435
```

---

## Variance Formula

```text
Variance %

=
(Actual - Benchmark)
÷
Benchmark
```

---

## Example

```text
Actual

=
9,358
```

```text
Benchmark

=
9,435
```

```text
Variance

=
-0.8%
```

---

# Section 3 - Forecast Methodology

## Method

Historical Ratio Forecasting

Also Known As:

```text
# Foster Program Dashboard Functional Specification
## Appendix and Final Dashboard Summary

### Purpose

This appendix provides a consolidated view of the completed Foster Program Dashboard solution.

It summarizes:

- Business objectives
- Dashboard structure
- Page purposes
- KPI framework
- Benchmark methodology
- Forecast methodology
- Governance standards

This section serves as the executive overview for future analysts, developers, program owners, and AI agents.

---

# Dashboard Mission

The Foster Program Dashboard is a decision-support system designed to help leadership and program owners:

```text
Understand historical performance

Monitor current activity

Plan operational capacity

Forecast future participation

Support evidence-based decisions
```

The dashboard focuses on:

```text
Understanding

Decision Support

Planning

Transparency
```

rather than simple reporting.

---

# Target Audience

## Primary Audience

Program Owners

Primary Responsibilities:

```text
Operational Planning

Capacity Planning

Volunteer Recruitment

Resource Allocation

Performance Monitoring
```

---

## Secondary Audience

Directors

Primary Responsibilities:

```text
Strategic Planning

Program Oversight

Forecast Monitoring

Resource Governance
```

---

# Dashboard Architecture

## Page 1

Executive Summary

Purpose:

```text
What happened?

What should leadership know?
```

Questions Answered:

```text
Q1 Is the Program Growing?

Q2 Is There Seasonality?

Q3 What Is Happening Recently?

Q4 What Should We Expect Next?
```

Outcome:

```text
A complete executive narrative.
```

---

## Page 2

Program Lifecycle Analysis

Purpose:

```text
How did the program evolve?
```

Lifecycle Story:

```text
Expansion
↓
Peak Activity
↓
Stable Operations
↓
Mature Program
```

Outcome:

```text
Historical context and strategic understanding.
```

---

## Page 3

Capacity Planning & Seasonal Operations

Purpose:

```text
How should Program Owners prepare?
```

Focus:

```text
Seasonality

Benchmarks

Operational Planning

Capacity Management
```

Outcome:

```text
Planning guidance and operational readiness.
```

---

## Page 4

Methodology & Governance

Purpose:

```text
How were results calculated?
```

Focus:

```text
Definitions

Benchmarks

Forecasts

Validation

Governance
```

Outcome:

```text
Trust and reproducibility.
```

---

# Core Dashboard Narrative

The dashboard tells a single business story.

---

## Finding 1

Historical Growth

```text
Program Growth Since 2006

+144.6%
```

Most growth occurred during:

```text
2006-2010
```

Expansion Phase.

---

## Finding 2

Mature Operating Model

Recent participation has remained stable.

Recent Activity:

```text
Approximately 13,000-14,000
fostered animals annually.
```

Interpretation:

```text
Program is mature rather than expanding rapidly.
```

---

## Finding 3

Predictable Seasonality

Participation follows a repeatable seasonal cycle.

Peak Activity:

```text
May-August
```

Highest Month:

```text
June
```

Lowest Month:

```text
February
```

Interpretation:

```text
Seasonality is predictable and supports proactive planning.
```

---

## Finding 4

Stable Short-Term Outlook

2026 Projection:

```text
12,800
```

2025 Actual:

```text
12,778
```

Interpretation:

```text
2026 is expected to finish near 2025 levels.
```

---

# KPI Framework Summary

## KPI 1

Historical Foster Animals

Question:

```text
How large is the program?
```

Value:

```text
109,738
```

---

## KPI 2

Program Growth Since 2006

Question:

```text
How much has the program grown?
```

Value:

```text
+144.6%
```

---

## KPI 3

August 2026 Performance

Question:

```text
Did the latest month meet expectations?
```

Value:

```text
9,358
```

Benchmark:

```text
9,435
```

Status:

```text
On Target
```

---

## KPI 4

2026 YTD Fostered Animals

Question:

```text
How is the year progressing?
```

Value:

```text
11,312
```

Benchmark:

```text
11,840
```

Status:

```text
Slightly Below Benchmark
```

---

## KPI 5

2026 Year-End Projection

Question:

```text
What should we expect next?
```

Value:

```text
12,800
```

Forecast Accuracy:

```text
±0.9%
```

Status:

```text
Projected to Finish Near 2025 Levels
```

---

# Benchmark Methodology Summary

Benchmark Type:

```text
5-Year Historical Average
```

Period:

```text
2021-2025
```

Purpose:

```text
Represent expected performance
during the current mature era.
```

Formula:

```text
Average(
2021,
2022,
2023,
2024,
2025
)
```

Used For:

```text
Monthly Performance

YTD Analysis

Operational Planning
```

---

# Forecast Methodology Summary

Method:

```text
Historical Ratio Forecasting
```

Also Known As:

```text
Completion Rate Forecasting
```

Purpose:

```text
Estimate year-end participation.
```

---

## Process

Step 1

Calculate:

```text
Historical Completion Rates
```

---

Step 2

Calculate:

```text
Average Completion Rate

88.5%
```

---

Step 3

Apply:

```text
Current YTD
÷
88.5%
```

---

Result:

```text
2026 Projection

12,800
```

---

# Forecast Validation Summary

Backtesting Results:

| Year | Error |
|--------|--------:|
| 2021 | -0.9% |
| 2022 | -0.6% |
| 2023 | -0.5% |
| 2024 | +1.8% |
| 2025 | +0.6% |

Average Historical Error:

```text
±0.9%
```

Interpretation:

```text
Forecast performs well against historical results.
```

---

# Semantic Model Standards

Primary Reporting Metric:

```text
Fostered Animals
```

Definition:

```text
Distinct animals that experienced foster care
during the selected reporting period.
```

Measure:

```text
DISTINCTCOUNT(
    'Foster Animal Month'[SB Animal ID]
)
```

Reason:

```text
Represents program reach and participation.

Supports:
- Trends
- Growth
- Seasonality
- Forecasting
- Executive Reporting
```

---

# Dashboard Design Standards

Every page follows:

```text
Business Question
↓
Executive Answer
↓
Evidence
↓
Insight
↓
Decision Support
```

Every visual follows:

```text
What should the user understand in 5 seconds?
```

Every KPI follows:

```text
Question
↓
Metric
↓
Benchmark
↓
Variance
↓
Status
↓
Action
```

---

# Governance Standards

Every metric must include:

```text
Definition

Calculation

Business Purpose

Owner
```

Every forecast must include:

```text
Method

Assumptions

Validation

Accuracy
```

Every enhancement request must answer:

```text
What business question does this support?
```

before development begins.

---

# Final Dashboard Conclusion

The Foster Program Dashboard demonstrates that:

```text
The Foster Program experienced rapid expansion
between 2006 and 2010 before transitioning into
a mature and sustainable operating model.

Participation remains stable, seasonal patterns
remain highly predictable, and historical forecasting
suggests 2026 will finish near 2025 levels.

The dashboard provides Program Owners and Directors
with a trusted decision-support system for monitoring
performance, planning capacity, and forecasting future
participation using transparent and validated methods.
```

---

# Final Principle

```text
Do not build dashboards.

Build decision-support systems.
```

The dashboard is not the outcome.

Better decisions are the outcome.

# End of Foster Program Dashboard Functional Specification