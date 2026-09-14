# Fabric Data Coach Part 1 - Executive Analytics Framework Playbook
## From Dashboard Development to Decision Support Systems

### Purpose
This framework documents the repeatable process we developed while building the Foster Program Executive Dashboard. The goal is not to build dashboards faster. The goal is to build dashboards that:
* Answer business questions
* Support decisions
* Explain findings
* Build trust in the data
* Create repeatable analytical processes

---

### The Biggest Lesson
Most BI projects follow this process:
1. Data
2. ↓
3. Dashboard

This often creates:
* KPI overload
* Unused reports
* Confusing visuals
* No decision-making value

Instead, we used:
1. Data
2. ↓
3. Exploratory Data Analysis
4. ↓
5. Business Questions
6. ↓
7. Key Findings
8. ↓
9. Executive Story
10. ↓
11. KPI Design
12. ↓
13. Benchmarks
14. ↓
15. Forecasting
16. ↓
17. UX Design
18. ↓
19. Dashboard

The dashboard becomes the final output of analysis rather than the starting point.

---

### The Decision-First Analytics Framework
Every report should be built using the following sequence.

#### Step 1 - Understand The Data
Before creating KPIs or visuals:
* **Ask:** What is the data actually telling us?
* **Activities:** 
  * Exploratory Data Analysis
  * Trend Analysis
  * Seasonality Analysis
  * Distribution Analysis
  * Benchmark Analysis
  * Outlier Detection

The objective is to discover the story before designing the report.

#### Step 2 - Challenge Assumptions
Do not assume the business narrative is correct.
* **Initial assumption:** The Foster Program is growing.
* **Data revealed:** 
  1. Rapid growth (2006-2010)
  2. ↓
  3. Long-term stabilization
  4. ↓
  5. Mature operating model

The analysis changed the story. Always let the data decide the narrative.

#### Step 3 - Define Executive Questions
Before creating visuals, define the questions leadership actually wants answered.
* **Q1:** Is the Program Growing?
* **Q2:** Is There Seasonality?
* **Q3:** What Is Happening Recently?
* **Q4:** What Should We Expect Next?

Every page and every visual must support one of these questions.

#### Step 4 - Develop The Executive Narrative
Before creating the report, write the story.
* **Example Narrative:** 
  > The Foster Program experienced rapid growth between 2006 and 2010 before transitioning into a mature and stable operating model. Activity now follows predictable seasonal patterns and recent participation levels remain consistent with historical expectations.

The dashboard should prove the narrative. The dashboard should not create the narrative.

#### Step 5 - Design KPIs Around Decisions
A KPI should never exist simply because the data exists. Every KPI must answer:
* **Question:** What decision does this KPI support?
* **Bad KPI:** Average Annual Volume
  * *Question answered:* None
* **Good KPI:** Program Growth Since 2006
  * *Question answered:* How much has the program grown?

---

### KPI Design Framework
For every KPI document:
* **KPI Name:** Program Growth Since 2006
* **Calculation:** $(2025 	ext{ Value} - 2006 	ext{ Value}) \div 2006 	ext{ Value}$
* **Business Question:** How much has the program grown?
* **Business Meaning:** Measures long-term expansion relative to the first reporting year.
* **Threshold:** Green / Yellow / Red
* **Action:** If growth falls significantly below expectations, review program participation drivers.

#### KPI Validation Rule
If a KPI cannot answer the question, **"Why should a manager care?"**, remove it. This simple rule eliminates most unnecessary KPIs.

---

### Executive Reporting Rule
Every page should contain:
* **Executive Question:** Is the Program Growing?
* **Executive Answer:** Historically yes. Recently stable.
* **Supporting Evidence:** Charts, KPIs, Benchmarks, Forecasts
* **Executive Insight:** A short paragraph that explains the conclusion.

---

### The First Principle
**Do not build dashboards. Build decision support systems.**

Everything else—KPIs, Charts, Forecasts, Heatmaps, Tables—are simply tools used to support decisions.

# Part 2 - Exploratory Data Analysis (EDA), Business Questions Framework, and KPI Design Methodology

## Purpose

Exploratory Data Analysis (EDA) is the most important phase of any analytics project.

Most dashboard projects skip EDA and immediately begin designing visuals.

This often results in:

- Incorrect assumptions
- Irrelevant KPIs
- Poor executive storytelling
- Dashboards with low business value

The purpose of EDA is to discover what the data actually says before designing a report.

---

# Exploratory Data Analysis (EDA) Framework

## Goal

Transform raw data into analytical findings.

The output of EDA is not a dashboard.

The output of EDA is:

```text
Insights
Patterns
Questions
Hypotheses
Narratives
```

which eventually become report content.

---

## EDA Process

### Step 1 - Understand The Grain

Before calculating anything:

Define:

```text
What does one row represent?
```

Examples:

Monthly Foster Participation

```text
One row = Animal + Month
```

Sales Dataset

```text
One row = Customer Purchase
```

Fact Table

```text
One row = Business Event
```

Never start analysis until the grain is understood.

---

### Step 2 - Validate Core Metrics

Questions:

```text
What does this measure mean?

How is it calculated?

Can it be trusted?

Does everyone interpret it the same way?
```

For the Foster Program project:

Metric:

```text
Fostered Animals
```

Definition:

```text
Distinct Count of Animal IDs
```

This became the foundation for every KPI.

---

### Step 3 - Look For Patterns

Typical analysis areas:

### Trend Analysis

Examples:

```text
Growing
Declining
Stable
```

---

### Seasonality Analysis

Examples:

```text
Monthly Peaks
Monthly Troughs
Predictable Demand Cycles
```

---

### Variance Analysis

Examples:

```text
Current vs Historical
Current vs Benchmark
Current vs Prior Year
```

---

### Distribution Analysis

Examples:

```text
Month
Year
Region
Program
Category
```

---

### Forecastability Analysis

Examples:

```text
Can behavior be predicted?

Are patterns stable?

Can historical ratios be used?
```

---

## Challenge Assumptions

One of the most important exercises.

Example:

Business Assumption:

```text
Program is growing.
```

EDA Finding:

```text
Rapid growth occurred between 2006 and 2010.

Growth stabilized after 2010.

Program is now mature.
```

Conclusion:

```text
Data changed the business story.
```

Always trust the data before opinions.

---

# Business Question Framework

## Purpose

Most dashboards become collections of unrelated charts.

Instead, build reports around business questions.

---

## Question Design Process

Ask:

```text
What decisions are users trying to make?
```

Examples:

### Executive Level

```text
Is the Program Growing?

Is There Seasonality?

What Is Happening Recently?

What Should We Expect Next?
```

---

### Operational Level

```text
When should we increase capacity?

Which month requires the most resources?

How are we performing against benchmark?
```

---

### Strategic Level

```text
How has the program evolved?

Are we operating within a mature range?

What should future planning assume?
```

---

## Dashboard Rule

Every page should answer one primary question.

Every visual should support that question.

If a visual does not support a business question:

Remove it.

---

# Business Question Template

For every report page document:

### Question

Example:

```text
Is There Seasonality?
```

---

### Executive Answer

Example:

```text
Yes.

Peak activity consistently occurs between May and August.
```

---

### Evidence

Examples:

```text
Area Chart

Benchmark Table

Seasonality Heatmap
```

---

### Insight

Explain:

```text
Why does this matter?
```

---

# KPI Design Methodology

## KPI Design Philosophy

A KPI is not a number.

A KPI is a decision-support tool.

---

# KPI Design Framework

For every KPI document:

### KPI Name

Example:

```text
Program Growth Since 2006
```

---

### Calculation

Example:

```text
(2025 Value - 2006 Value)
÷
2006 Value
```

---

### Business Question

Example:

```text
How much has the program grown?
```

---

### Business Meaning

Example:

```text
Measures long-term expansion relative to the first reporting year.
```

---

### Benchmark

Example:

```text
Historical Performance
```

---

### Threshold

Example:

```text
Green
Yellow
Red
```

---

### Action

Example:

```text
Investigate unexpected declines.
```

---

# KPI Validation Test

Every KPI must pass all five questions.

### Question 1

```text
What business question does this KPI answer?
```

---

### Question 2

```text
What action does this KPI drive?
```

---

### Question 3

```text
What benchmark is used?
```

---

### Question 4

```text
What threshold determines success or concern?
```

---

### Question 5

```text
Can a user understand this KPI in less than 5 seconds?
```

---

# KPI Examples

## Weak KPI

```text
Average Annual Volume
```

Problems:

- No decision attached
- No benchmark
- No action
- No threshold

---

## Strong KPI

```text
Program Growth Since 2006

+144.6%
```

Question Answered:

```text
How much has the program grown?
```

Benchmark:

```text
2006 Participation Level
```

Provides:

```text
Historical Context
```

---

## Strong KPI

```text
August 2026 Performance

9,358
```

Supports:

```text
What happened most recently?
```

Includes:

```text
Actual

Benchmark

Variance

Status
```

Making it actionable.

---

# KPI Layering Strategy

Every executive dashboard should contain multiple KPI layers.

### Historical

Example:

```text
Historical Foster Animals
```

---

### Growth

Example:

```text
Program Growth Since 2006
```

---

### Current Performance

Example:

```text
August 2026 Performance
```

---

### Year-To-Date

Example:

```text
2026 YTD Fostered Animals
```

---

### Future Outlook

Example:

```text
2026 Year-End Projection
```

---

# End Of Part 2

Next Block:

Part 3 - Benchmark Framework, Forecasting Framework, Forecast Validation Methodology, and KPI Threshold Design
# Part 3 - Benchmark Framework, Forecasting Framework, Forecast Validation Methodology, and KPI Threshold Design

## Purpose

Most dashboards fail because they display values without context.

A number by itself does not support a decision.

Example:

```text
9,358 Foster Animals
```

Leadership immediately asks:

```text
Is that good or bad?

Is that expected?

What action should I take?
```

This is why benchmarks, thresholds, and forecasting methodologies are critical.

They convert numbers into decision-support information.

---

# Benchmark Framework

## Purpose

A benchmark represents expected performance.

Benchmarks allow users to determine:

```text
Above Expectations

At Expectations

Below Expectations
```

without requiring interpretation.

---

# Benchmark Design Principles

A benchmark should be:

### Objective

Based on data.

Avoid:

```text
Manager opinions
```

---

### Repeatable

The same calculation should always produce the same result.

---

### Explainable

Business users should understand how the benchmark was derived.

---

### Governable

Future analysts must be able to reproduce the benchmark.

---

# Recommended Benchmark Method

## 5-Year Historical Average

Formula:

```text
Benchmark

=
Average(
Year 1,
Year 2,
Year 3,
Year 4,
Year 5
)
```

Example:

### August Benchmark

Historical Values:

```text
2021 = 9,285

2022 = 9,473

2023 = 9,636

2024 = 9,417

2025 = 9,362
```

Calculation:

```text
(9,285
+9,473
+9,636
+9,417
+9,362)

÷ 5

=
9,435
```

August Benchmark:

```text
9,435
```

---

# Variance Framework

## Purpose

Measure 
# Part 4 - UX Design Framework, Data Storytelling, AI Collaboration Workflow, and Reusable System Prompt

## Purpose

Technical accuracy alone does not create a successful dashboard.

Many reports contain accurate data but fail because users cannot quickly understand:

- What happened?
- Why it happened?
- Whether action is required?
- What should happen next?

The purpose of UX Design and Data Storytelling is to transform analysis into understanding.

---

# UX Design Philosophy

## The 5-Second Rule

Every visual should answer:

```text
What should the user understand in 5 seconds?
```

If the answer is unclear, redesign the visual.

---

## Bad Dashboard Design

User sees:

```text
Chart
Table
Cards
KPIs
```

User asks:

```text
What am I supposed to learn?
```

---

## Good Dashboard Design

User sees:

```text
Question
↓
Answer
↓
Evidence
↓
Action
```

The learning path becomes obvious.

---

# Data Storytelling Framework

The job of a dashboard is not to display data.

The job of a dashboard is to tell a business story.

---

## Story Structure

Every page should follow:

### Executive Question

Example:

```text
Is the Program Growing?
```

---

### Executive Answer

Example:

```text
Historically yes.

Recently stable.
```

---

### Evidence

Examples:

```text
Charts

KPIs

Benchmarks

Forecasts
```

---

### Executive Insight

Example:

```text
Most growth occurred between 2006 and 2010.

Recent activity has stabilized, indicating a mature operating model.
```

---

# The Visual Storytelling Hierarchy

Every visual should contain four layers.

---

## Layer 1

Question

Example:

```text
Q1. Is the Program Growing?
```

Immediately tells the user why the visual exists.

---

## Layer 2

Visual Evidence

Examples:

```text
Line Charts

Area Charts

Heatmaps

Bar Charts
```

Used to support the answer.

---

## Layer 3

Annotations

Examples:

```text
Peak Year

Lowest Month

Expansion Phase

Forecast
```

Annotations explain why the visual matters.

---

## Layer 4

Executive Insight

A short narrative that summarizes the finding.

This becomes the business conclusion.

---

# Visual Design Principles

## Rule 1

Show the story.

Do not show every number.

Example:

Instead of labeling every year:

```text
2006
2007
2008
...
2025
```

Label:

```text
First Year

Peak Year

Latest Year
```

Focus attention on what matters.

---

## Rule 2

Use annotations.

Examples:

```text
Peak Year

Expansion Phase

Highest Month

Forecast Point
```

Annotations help users understand why a point matters.

---

## Rule 3

Use visual grouping.

Examples:

```text
Expansion Phase
2006-2010
```

```text
Mature Phase
2011-2025
```

Grouping tells a stronger story than individual values.

---

## Rule 4

Use benchmarks.

A number alone has little meaning.

Example:

```text
Actual = 9,358
```

Better:

```text
Actual = 9,358

Benchmark = 9,435

Variance = -0.8%

On Target
```

Context creates understanding.

---

## Rule 5

Separate strategic and operational information.

Strategic:

```text
Long-Term Growth

Lifecycle Analysis

Forecasts
```

Operational:

```text
Monthly Benchmarks

Capacity Planning

Current Performance
```

Mixing both often creates confusion.

---

# Dashboard Layout Strategy

## Executive Summary First

Always begin with:

```text
What executives need to know.
```

Not:

```text
How calculations work.
```

---

## Suggested Dashboard Flow

### Page 1

Executive Summary

Answers:

```text
What happened?

What should leadership know?
```

---

### Page 2

Strategic Analysis

Answers:

```text
How did we get here?
```

---

### Page 3

Operational Planning

Answers:

```text
What should program owners do?
```

---

### Page 4

Methodology

Answers:

```text
How was everything calculated?
```

---

# Data Storytelling Example

## Poor Story

```text
Growth Chart

Seasonality Chart

Forecast Chart
```

No connection.

---

## Strong Story

```text
Program Grew Rapidly

↓

Program Stabilized

↓

Seasonality Exists

↓

2026 Tracking Normally

↓

Forecast Remains Stable
```

Every page supports the next conclusion.

---

# How We Used AI

This project did not use AI as a report generator.

Instead AI acted as:

---

## Data Analyst

Responsibilities:

```text
Analyze data

Validate findings

Challenge assumptions

Identify trends
```

---

## Business Analyst

Responsibilities:

```text
Define business questions

Identify decision points

Translate findings into business language
```

---

## Statistician

Responsibilities:

```text
Build benchmark methodology

Validate forecasting

Backtest forecast accuracy
```

---

## BI Consultant

Responsibilities:

```text
Design KPI framework

Define thresholds

Review dashboard structure
```

---

## UX Designer

Responsibilities:

```text
Improve readability

Reduce clutter

Strengthen storytelling

Design executive experiences
```

---

## Executive Advisor

Responsibilities:

```text
Convert analysis into decisions

Identify executive takeaways

Refine conclusions
```

---

# AI Collaboration Framework

Before asking AI to build visuals:

Step 1

```text
Perform Exploratory Data Analysis
```

---

Step 2

```text
Develop findings
```

---

Step 3

```text
Create business questions
```

---

Step 4

```text
Design KPIs
```

---

Step 5

```text
Create benchmarks
```

---

Step 6

```text
Validate forecasts
```

---

Step 7

```text
Design visuals
```

---

Step 8

```text
Build dashboard
```

---

# Reusable AI System Prompt

Use this prompt at the beginning of future projects.

You are a Senior Data Analyst, Business Analyst, Statistician, BI Consultant, Executive Reporting Advisor, and UX Designer.

Your purpose is to build decision-support systems rather than dashboards.

Always follow this workflow:

1. Understand the data.
2. Perform exploratory data analysis.
3. Challenge assumptions.
4. Define business questions.
5. Identify key insights.
6. Design KPIs that answer business questions.
7. Build benchmark methodologies.
8. Validate forecasting methodologies.
9. Design visuals that communicate findings.
10. Create executive insights.
11. Build the final dashboard.

For every KPI define:

- Calculation
- Business Meaning
- Benchmark
- Threshold
- Action

For every visual answer:

"What should the user understand within 5 seconds?"

For every page define:

- Executive Question
- Executive Answer
- Evidence
- Executive Insight

Never start with visuals.

Start with analysis.

Always think as:

Data Analyst
↓
Business Analyst
↓
Statistician
↓
UX Designer
↓
BI Developer

The dashboard should tell a coherent business story that supports decision making.

---

# Final Principle

The most important lesson from this project:

```text
Do not build dashboards.

Build decision support systems.
```

Charts, KPIs, forecasts, tables, benchmarks, and heatmaps are simply the tools used to communicate decisions.

A successful dashboard is one that helps people make better decisions faster.

# End Of Part 4
# Part 5 - Developer Handoff Framework and Lessons Learned

## Purpose

The final deliverable of an analytics project should not be a dashboard.

The final deliverable should be:

```text
A repeatable decision-support system
```

that can be maintained, scaled, validated, and enhanced by other analysts and developers.

This section defines how analytical work should be transferred from analysis into implementation.

---

# Developer Handoff Framework

## Goal

Ensure a developer can build the report without needing to rediscover business logic.

A successful handoff should provide:

- Business Questions
- KPI Definitions
- Calculations
- Benchmarks
- Thresholds
- Forecasting Methodology
- UX Requirements
- Executive Narrative

---

# Handoff Package Structure

Every reporting project should produce the following artifacts.

---

## Artifact 1

Executive Questions

Example:

### Page 1

Question:

```text
Is the Program Growing?
```

Executive Answer:

```text
Historically yes.

Recently stable.
```

---

### Page 2

Question:

```text
How has the program evolved?
```

Executive Answer:

```text
Expansion
↓
Peak
↓
Stable
↓
Mature
```

---

### Page 3

Question:

```text
How should Program Owners plan for seasonal demand?
```

Executive Answer:

```text
Peak activity occurs between May and August.

Plan capacity before May.
```

---

## Artifact 2

KPI Specification

Every KPI should include:

### KPI Name

Example:

```text
Program Growth Since 2006
```

---

### Business Question

```text
How much has the program grown?
```

---

### Calculation

```text
(2025 - 2006)
÷
2006
```

---

### Benchmark

```text
2006 Baseline
```

---

### Threshold

```text
Green
Yellow
Red
```

---

### Action

```text
Investigate significant decline.
```

---

## Artifact 3

Benchmark Specification

Document:

### Benchmark Type

Example:

```text
5-Year Monthly Benchmark Average
```

---

### Data Sources

Example:

```text
2021
2022
2023
2024
2025
```

---

### Formula

Example:

```text
Average(
2021,
2022,
2023,
2024,
2025
)
```

---

## Artifact 4

Forecasting Specification

Document:

### Method

```text
Historical Ratio Forecasting
```

---

### Inputs

```text
August YTD

Historical Completion Rate
```

---

### Formula

```text
Current YTD
÷
Historical Completion Rate
```

---

### Validation

```text
Historical Forecast Error

±0.9%
```

---

## Artifact 5

Visual Specifications

For every visual define:

### Business Question

### Visual Type

### Data Source

### Annotations

### Executive Insight

### Intended User Takeaway

This prevents developers from creating technically correct but analytically weak visuals.

---

# Dashboard Development Process

Recommended workflow:

```text
Business Questions
↓
Analysis
↓
Insights
↓
KPIs
↓
Benchmarks
↓
Forecasts
↓
Visual Specifications
↓
Power BI Development
```

Never reverse this order.

---

# Report Validation Checklist

Before publishing:

### Data Validation

Questions:

```text
Are calculations correct?

Are benchmarks reproducible?

Are definitions documented?
```

---

### KPI Validation

Questions:

```text
Does every KPI answer a business question?

Does every KPI support a decision?

Does every KPI contain context?
```

---

### Forecast Validation

Questions:

```text
Was the forecast tested?

Can forecast accuracy be explained?

Can the methodology be reproduced?
```

---

### UX Validation

Questions:

```text
Can users understand the story within 5 seconds?

Does every visual have a purpose?

Does the page answer its question?
```

---

# Lessons Learned From The Foster Program Project

## Lesson 1

Data Changed The Narrative

Original assumption:

```text
Program is growing.
```

Actual finding:

```text
Rapid growth occurred between 2006 and 2010.

Program later stabilized.

Program is now mature.
```

Never assume the story before analysis.

---

## Lesson 2

Executive Questions Are More Valuable Than Charts

We achieved significantly better outcomes after defining:

```text
Q1

Is the Program Growing?

Q2

Is There Seasonality?

Q3

What Is Happening Recently?

Q4

What Should We Expect Next?
```

The questions drove the design.

The charts became supporting evidence.

---

## Lesson 3

Every KPI Must Support A Decision

Many traditional KPI dashboards contain:

```text
Interesting metrics
```

but not:

```text
Decision metrics
```

Good KPIs answer:

```text
What action should someone take?
```

---

## Lesson 4

Benchmarks Matter More Than Raw Values

A value alone provides limited meaning.

Example:

```text
9,358 Foster Animals
```

has little context.

Example:

```text
9,358

Benchmark = 9,435

Variance = -0.8%

On Target
```

supports decision-making.

---

## Lesson 5

Forecasts Must Be Tested

Forecasts become much more credible when validated.

Example:

```text
2026 Projection

12,800
```

became significantly stronger after demonstrating:

```text
Historical Forecast Error

±0.9%
```

Trust increases when assumptions are tested.

---

## Lesson 6

UX Matters More Than Most Analysts Realize

A correct chart can still fail.

The dashboard improved dramatically when we:

- Added annotations
- Added phase labels
- Added benchmarks
- Added executive insights
- Removed unnecessary detail

The objective is understanding, not visualization.

---

## Lesson 7

AI Works Best As A Thought Partner

The most successful use of AI was not:

```text
Build me a dashboard.
```

The most successful use was:

```text
Challenge assumptions.

Review findings.

Design questions.

Validate methodology.

Improve storytelling.
```

AI performed best when treated as:

- Data Analyst
- Business Analyst
- Statistician
- UX Designer
- BI Consultant

rather than a report generator.

---

# Final Framework Summary

```text
Data
↓
Exploratory Data Analysis
↓
Challenge Assumptions
↓
Business Questions
↓
Insights
↓
Executive Narrative
↓
KPIs
↓
Benchmarks
↓
Forecasting
↓
Validation
↓
Visual Design
↓
Dashboard
↓
Decision Support
```

---

# Final Principle

The most important lesson from this project:

Do not build reports.

Do not build dashboards.

Build decision-support systems.

When analysis, KPI design, forecasting, UX, and storytelling are aligned, the dashboard becomes a tool that helps leaders make better decisions rather than a collection of charts.

# End Of Playbook
# Part 6 - Appendix, Reference Materials, Templates, and Reusable Assets

## Purpose

The Appendix contains reusable templates, reference models, checklists, and supporting materials that can be reused across future analytics, BI, Fabric, and Power BI projects.

The objective is to avoid reinventing the process for every report.

---

# Appendix A - Executive Question Template

Every dashboard should begin with executive questions.

Do not begin with charts.

Do not begin with KPIs.

Begin with business decisions.

---

## Executive Question Template

### Question

```text
What business question are we trying to answer?
```

---

### Executive Answer

```text
What is the shortest possible answer?
```

---

### Supporting Evidence

```text
Which KPIs support the answer?

Which visuals support the answer?

Which benchmarks support the answer?
```

---

### Executive Insight

```text
Why does this finding matter?
```

---

## Example

### Question

```text
Is the Program Growing?
```

### Executive Answer

```text
Historically yes.

Recently stable.
```

### Evidence

```text
Historical Trend

Growth KPI

CAGR Analysis
```

### Executive Insight

```text
Growth occurred primarily between 2006 and 2010.

Recent activity reflects a mature operating model.
```

---

# Appendix B - KPI Design Template

Every KPI should follow the same structure.

---

## KPI Specification Template

### KPI Name

```text
Program Growth Since 2006
```

---

### Business Question

```text
How much has the program grown?
```

---

### Business Meaning

```text
Measures long-term growth relative to the first reporting year.
```

---

### Calculation

```text
(Current Year - Baseline Year)
÷
Baseline Year
```

---

### Benchmark

```text
Historical Baseline
```

---

### Threshold

```text
Green

Yellow

Red
```

---

### Action

```text
Review declining participation trends