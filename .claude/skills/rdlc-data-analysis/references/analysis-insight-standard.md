# Analysis Insight Standard v1.0

## 1. Purpose

This standard defines the decisions and requirements that must be settled before an RDLC report, dashboard, analytical pack, or recurring insight product is designed. It bridges the gap between a governed business definition and a finished analysis product.

The standard is used during Step 0 intake and during artifact generation. It ensures the interview establishes not only what to measure, but also:

- what comparisons make the result meaningful;
- which analytical views are required;
- how changes, exceptions, and drivers will be identified;
- what interpretation is permitted;
- what evidence supports each published insight; and
- how the completed analysis will lead to a business action.

This standard does not replace the BRD Content Standard, KPI Contracts, metric definitions, data-quality specifications, or technical design. It defines the analysis layer that connects those artifacts to the final product.

## 2. Governing principles

### 2.1 Decision before visual

Do not select a chart, page, ranking, alert, or forecast until the business question and intended action are settled.

### 2.2 Definition before comparison

Do not compare groups or periods until the measure, population, grain, dimensions, and time rules are governed.

### 2.3 Context before interpretation

A value must be shown with the context needed to interpret it, including the applicable denominator, volume, comparison period, completeness, and known limitations.

### 2.4 Descriptive evidence is not causal evidence

A difference, trend, or association does not establish why it occurred. Do not claim causation unless the analysis design and evidence support it.

### 2.5 No target without authority

Historical averages, percentiles, forecast values, and peer results must not be presented as targets unless the responsible stakeholder approves them as targets.

### 2.6 No ranking without safeguards

Rankings require comparable populations, compatible definitions, sufficient volume, and explicit handling of Unknown, Unmapped, suppressed, and incomplete results.

### 2.7 Every insight needs evidence

A published narrative statement must be traceable to a governed measure, comparison, filter context, and validation result.

## 3. When to apply this standard

Apply this standard when the requested artifact includes one or more of the following:

- historical trend analysis;
- current-period monitoring;
- comparisons across locations, programs, populations, or products;
- seasonality;
- driver analysis;
- rankings;
- exception detection;
- forecasting or projections;
- targets or status indicators;
- narrative insights;
- executive summaries;
- recurring PDF or Power BI reporting; or
- data-quality reporting.

For a simple extract or operational list, use only the applicable sections.

## 4. Analysis insight design tree

The intake must consider the following branches. Ask only frontier decisions whose prerequisites are settled. Retrieve facts where possible rather than asking the stakeholder to recall them.

### 4.1 Decision and action

Settle:

- the decision the analysis supports;
- the action a consumer may take;
- the decision owner;
- the review frequency;
- why timing matters;
- the level of authority of the consumer; and
- what the analysis must not be used to decide.

Required output:

```text
Decision supported:
Intended action:
Decision owner:
Review frequency:
Interpretation boundary:
```

### 4.2 Business questions

For each business question define:

- the question in plain language;
- the measure or evidence used to answer it;
- the comparison required;
- the analysis grain;
- the consumer;
- the expected action; and
- whether the question is answered, partially answered, deferred, or out of scope.

A question that does not affect a decision, action, governance requirement, or acceptance test should not drive a report view.

### 4.3 Measure context

Before analysis design, confirm for every measure:

- business definition;
- formula;
- numerator and denominator;
- population;
- grain;
- dimensions;
- time zone;
- period boundaries;
- inclusions and exclusions;
- distinct-count rules;
- additive or non-additive behaviour;
- Unknown and Unmapped treatment;
- owner;
- refresh frequency; and
- validation source.

If these are not settled, return to the KPI or metric-definition branch before designing insights.

### 4.4 Comparison framework

Settle which comparisons are meaningful and authorized.

Potential comparison types:

- current period versus prior period;
- current period versus same period last year;
- year to date versus prior year to date;
- rolling average;
- recent baseline;
- historical range;
- approved target;
- forecast;
- peer group;
- geographic group;
- population segment; and
- program phase.

For each comparison define:

- comparison population;
- comparison period;
- completeness requirement;
- calendar alignment;
- treatment of partial periods;
- treatment of source-system changes;
- treatment of organizational reorganizations;
- minimum volume; and
- interpretation boundary.

Do not compare groups that use different definitions, incompatible denominators, materially different data coverage, or unresolved historical mappings.

### 4.5 Time analysis

When time is material, settle:

- earliest reporting period;
- latest eligible period;
- monthly, quarterly, annual, or other grain;
- complete-period rule;
- open or immature record treatment;
- calendar versus fiscal reporting;
- seasonality window;
- rolling-window length;
- historical phase breaks;
- source-system or policy change markers;
- late-arriving record treatment; and
- historical-restatement policy.

Required time views may include:

- long-term trend;
- recent trend;
- monthly pattern;
- year-over-year change;
- year-to-date change;
- seasonality profile;
- program-phase comparison; and
- data-completeness timeline.

### 4.6 Driver dimensions

For each proposed dimension determine:

- why the dimension matters to the business decision;
- source authority;
- effective-date rule;
- hierarchy;
- normalization or grouping;
- Unknown and Unmapped treatment;
- reorganization or renaming treatment;
- minimum volume;
- privacy implications;
- whether totals are additive; and
- whether the dimension supports action or only description.

Examples include:

- Region and Centre;
- animal or client population;
- species or product family;
- program;
- channel;
- status;
- age or tenure band;
- source category; and
- service pathway.

Do not include a dimension merely because it is available in the source.

### 4.7 Volume, rate, duration, and intensity

Determine which analytical perspectives are required:

- **Volume**: how many entities or events occurred;
- **Rate or share**: how common the outcome is relative to an eligible population;
- **Duration**: how long the state or process lasted;
- **Frequency**: how many events occurred per entity or period;
- **Intensity**: how much service or activity occurred;
- **Mix**: how the total is distributed across categories; and
- **Change**: how the result differs from a valid comparison.

A rate should normally be shown with numerator and denominator. A duration should identify censoring and open-record treatment. A distinct count should disclose non-additivity when entities can belong to multiple groups.

### 4.8 Trend and seasonality

Before describing trend or seasonality, settle:

- minimum historical coverage;
- complete-period requirement;
- calendar alignment;
- structural breaks;
- smoothing method, if any;
- seasonal baseline period;
- treatment of extraordinary periods;
- minimum observations; and
- whether seasonality is descriptive or used for forecasting.

Recommended default views when supported by the data:

1. complete-history annual trend;
2. recent five-year monthly trend;
3. month-of-year seasonal profile;
4. year-over-year percentage and percentage-point change; and
5. annotated program or source-system phases.

These are defaults only. The stakeholder must settle any decision-sensitive baseline or period.

### 4.9 Current-period monitoring

If current monitoring is required, define:

- current-period cutoff;
- latest complete period;
- incomplete-period display;
- comparison baseline;
- status logic;
- threshold authority;
- alert owner;
- expected action;
- latency tolerance; and
- data-freshness indicator.

Do not label results On Target, Action Required, Critical, or similar unless the thresholds and expected actions are approved.

When thresholds are not approved, use neutral language such as:

- Higher utilization;
- Lower utilization;
- Above recent range;
- Below recent range;
- Material change after business review; and
- Data incomplete.

### 4.10 Ranking and segmentation

Before ranking, settle:

- primary ranking measure;
- supporting volume measure;
- direction of interpretation;
- minimum denominator or volume;
- ties;
- Unknown and Unmapped treatment;
- filtered versus unfiltered threshold application;
- peer grouping;
- suppression or descriptive-only rules; and
- whether best/worst language is allowed.

Rankings must not imply performance when the measure is descriptive.

Recommended safeguard structure:

```text
Sufficient volume: eligible for ranking
Low volume: value displayed, excluded from headline ranking
Very low volume: counts displayed, comparative rate suppressed
```

Threshold values require domain evidence or stakeholder approval.

### 4.11 Exceptions and signals

For every exception or signal define:

- triggering condition;
- business meaning;
- severity;
- owner;
- expected action;
- due date or review frequency;
- suppression rule;
- reset condition;
- data-quality dependency; and
- escalation path.

Differentiate:

- business exception;
- data-quality exception;
- source freshness issue;
- mapping issue;
- low volume;
- incomplete period; and
- expected seasonal variation.

### 4.12 Forecast and projection

Use forecasts only when they are necessary for the supported decision and the available history is suitable.

Settle:

- forecast target measure;
- forecast horizon;
- decision use;
- training window;
- seasonal treatment;
- known structural breaks;
- minimum history;
- back-testing method;
- error measure;
- prediction interval;
- refresh frequency;
- override policy; and
- presentation limitations.

Required validation should include rolling-origin or holdout back-testing where feasible. Suitable error measures may include MAE, RMSE, MAPE, weighted MAPE, or percentage-point error, chosen according to the measure and presence of zeros.

A projection must be labelled as a projection, not an actual. Do not present a single estimate without uncertainty when uncertainty can be quantified.

### 4.13 Narrative insights

For each narrative statement require:

- the governed measure;
- current value;
- comparison value;
- absolute or percentage-point difference;
- reporting period;
- filter context;
- volume or denominator;
- data-quality status;
- approved interpretation; and
- source or validation trace.

Narrative statements may describe:

- level;
- change;
- seasonality;
- concentration;
- distribution;
- exception;
- completeness; and
- forecast variance.

Narrative statements must not infer motive, causation, capacity, performance, or outcome unless governed evidence supports the inference.

### 4.14 Data-quality insights

Every recurring analysis product should identify the quality conditions that affect interpretation.

Potential signals:

- completeness;
- Unknown or Unmapped rate;
- duplicate rate;
- unresolved history;
- invalid interval;
- stale source;
- reconciliation variance;
- conflicting events;
- suppressed low volume;
- source coverage change; and
- historical restatement.

For each signal define the threshold, owner, business impact, and publication action.

### 4.15 Consumption and report architecture

Settle the required product views before choosing individual visuals.

A recurring analytical product may include:

1. **Executive summary**
   - principal measures;
   - current period;
   - important change;
   - comparison context;
   - quality status;
   - interpretation boundary.

2. **Historical trend**
   - complete-history view;
   - program phases;
   - annual or quarterly change.

3. **Recent monitoring**
   - current year or quarter;
   - same-period comparison;
   - latest complete period;
   - incomplete-period warning.

4. **Seasonality**
   - month-of-year pattern;
   - recent baseline;
   - peak and low periods;
   - seasonal limitation.

5. **Driver analysis**
   - location;
   - population;
   - program;
   - other approved dimensions;
   - volume and rate together.

6. **Intersection analysis**
   - two approved dimensions;
   - adequate volume safeguards;
   - non-additivity warning.

7. **Exceptions and investigation**
   - approved signal;
   - affected group;
   - expected action;
   - quality state.

8. **Methodology and data quality**
   - definitions;
   - formulas;
   - grain;
   - coverage;
   - limitations;
   - reconciliation;
   - refresh date.

9. **Restricted validation detail**
   - record-level traceability;
   - controlled access;
   - not included in a broadly distributed PDF unless approved.

Use only the views required by the confirmed questions and decisions.

## 5. Visual selection rules

Choose the visual after the analytical requirement is settled.

### 5.1 Recommended visual mappings

- Trend over time: line chart
- Actual versus comparison: line or clustered bar chart
- Seasonal month-by-year pattern: heat map
- Month-of-year seasonal profile: line chart
- Ranked categories: horizontal bar chart
- Composition: stacked bar when components are additive
- Rate with denominator: bar or dot plot with count in label or tooltip
- Distribution: histogram or box plot when statistically appropriate
- Two-dimensional intersection: matrix or heat map with volume safeguards
- Geography: map only when geographic position adds meaning
- KPI summary: card with current value, comparison, period, and quality indicator
- Forecast: actual and forecast line with prediction interval
- Data quality: scorecard plus exception table

### 5.2 Prohibited or discouraged patterns

Avoid:

- pie charts with many categories;
- dual axes that imply false relationships;
- truncated axes that exaggerate change without explicit justification;
- red/green status without text labels;
- rankings without denominators or volume context;
- cumulative totals when the question concerns period activity;
- adding non-additive distinct counts;
- maps used only as decoration;
- dense tables as the only executive view; and
- automated narrative that exceeds the governed evidence.

## 6. Insight specification

For each required insight or report view, record:

```text
Insight ID:
Business question:
Decision supported:
Consumer:
Expected action:
Primary measure:
Supporting measures:
Population:
Grain:
Dimensions:
Reporting period:
Comparison:
Minimum volume:
Quality gate:
Interpretation allowed:
Interpretation prohibited:
Recommended visual:
Narrative rule:
Validation evidence:
Owner:
Status:
```

This specification can be included in a BRD appendix or maintained as a separate Analysis Requirements artifact.

## 7. Acceptance criteria for analytical outputs

The analysis product is acceptable only when:

1. every required view answers a confirmed business question;
2. every displayed value traces to an approved definition;
3. comparison periods and populations are compatible;
4. partial and incomplete periods are labelled;
5. rates display or make available numerator and denominator;
6. distinct-count non-additivity is controlled;
7. low-volume safeguards are implemented;
8. Unknown, Unmapped, suppressed, and unavailable results are distinguishable from zero;
9. status labels use approved thresholds;
10. narrative statements reproduce from governed values;
11. forecast accuracy is validated when forecasting is included;
12. source-to-report reconciliation passes;
13. quality limitations are visible;
14. organization-wide totals are independently derived when subgroup totals are non-additive;
15. access to record-level validation detail is governed; and
16. the business owner accepts interpretation guidance and known limitations.

## 8. Step 0 interview additions

Add these potential decision areas to the RDLC design tree:

- analysis questions and expected actions;
- comparison framework;
- historical trend requirements;
- seasonality;
- current-period monitoring;
- driver dimensions;
- volume, rate, duration, frequency, and mix;
- ranking and low-volume safeguards;
- exception and signal logic;
- forecast and projection requirements;
- narrative-insight rules;
- report architecture;
- visual requirements;
- data-quality insights; and
- insight-level acceptance criteria.

Ask these only after their prerequisite metric, population, grain, and source decisions are settled.

## 9. Recommended interview sequence

The usual dependency order is:

1. Business problem
2. Decision and intended action
3. Business questions
4. Population and grain
5. Measure definitions
6. Time treatment
7. Dimensions and attribution
8. Comparison framework
9. Trend and seasonality
10. Rankings and safeguards
11. Signals and exceptions
12. Forecasting, if required
13. Report views and consumption
14. Narrative rules
15. Quality and validation
16. Acceptance and ownership

This is a dependency guide, not a mandatory questionnaire. Skip branches that cannot affect the authorized artifact.

## 10. Evidence and references

Facts should be retrieved from approved sources such as:

- prior BRDs;
- KPI Contracts;
- metric definitions;
- data-quality specifications;
- technical designs;
- semantic models;
- schemas and lineage;
- prior analytical packs;
- approved reports; and
- validation outputs.

A prior finished report may be used to identify reusable analysis patterns, but its thresholds, periods, visuals, forecasts, and conclusions must not be copied into a new product without confirming applicability.

## 11. Pre-delivery quality check

Before generating a BRD or analysis artifact, verify:

- the analysis supports the confirmed decision;
- every report view answers a confirmed question;
- every measure is governed;
- comparisons are compatible;
- the selected period is complete or clearly labelled;
- volume context accompanies rates;
- rankings have safeguards;
- targets and alerts have authority;
- forecasts have a decision use and validation method;
- narrative rules prohibit unsupported causal or performance claims;
- zeros are distinguished from missing or unavailable values;
- non-additivity is disclosed;
- quality limitations are visible;
- the final product includes methodology and validation evidence; and
- no analytical requirement was silently copied from a reference artifact.

## 12. Reusable analysis-pattern library

The following patterns may be proposed when prerequisites are satisfied. They are not automatically required.

### Pattern A: Historical baseline

Use when the organization needs a trusted long-term view before targets are approved.

Includes:

- complete-history trend;
- recent five-year view;
- year-over-year change;
- program phases;
- seasonality;
- definition and quality documentation.

### Pattern B: Current-period monitoring

Use when the organization needs recurring operational awareness.

Includes:

- latest complete period;
- year-to-date result;
- same-period prior-year comparison;
- freshness indicator;
- approved status logic or neutral comparison language.

### Pattern C: Driver analysis

Use when the organization needs to understand where or among which populations activity is concentrated.

Includes:

- governed dimensions;
- volume and rate together;
- intersection view;
- low-volume safeguards;
- Unknown and Unmapped reporting;
- non-additivity guidance.

### Pattern D: Data-quality assessment

Use when source reliability affects interpretation.

Includes:

- defect population;
- remediation rules;
- unresolved rate;
- completeness trend;
- source examples;
- publication gate;
- accepted limitations.

### Pattern E: Forecast and projection

Use when a future estimate changes a confirmed decision.

Includes:

- forecast horizon;
- baseline model;
- back-test;
- error measure;
- prediction interval;
- current projection;
- explicit uncertainty and limitation text.

### Pattern F: Executive analytical pack

Use when a recurring PDF must communicate findings without interactive exploration.

Includes:

- concise executive summary;
- key measures with comparison;
- annotated trend;
- driver view;
- quality indicator;
- methodology page;
- source and cutoff date;
- neutral, reproducible commentary.
