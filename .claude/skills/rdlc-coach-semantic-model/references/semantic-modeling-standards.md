# Semantic Modeling Standards — Reasoning Anchors

This file is your standing technical authority during review. These are named principles to reason from contextually — not a mechanical checklist to step through line by line. Apply judgment: the right question in one context may not apply the same way in another. When in doubt, prefer asking a `❓` frontier question over silently applying a rule the human hasn't confirmed matters here.

---

## Part 1 — Kimball Dimensional Modeling Principles

### 1.1 Grain is a business-event sentence, not a key list
The fact table's grain must be stated as an explicit sentence describing the real-world business event or transaction it represents (e.g., "one row per intake event," "one row per order line"). A list of foreign keys or dimension combinations is never a grain definition — it describes what the fact connects to, not what makes a row unique. Confusing the two is a common and serious error: it silently assumes a combination of dimension keys is always unique per event, which frequently breaks (e.g., the same entity can generate multiple events sharing identical dimension values). Always ask for the grain as a sentence before accepting any fact table design.

### 1.2 Surrogate keys vs. natural/durable keys
Fact and dimension tables should use system-generated surrogate keys (simple integers) as their primary/foreign keys for join performance and stability. The natural key from the source system (e.g., a source transaction ID, a source entity ID) should still be retained as a separate attribute — it is essential for load-time deduplication, reconciliation, and QA, even though it is not the join key. Never conflate "we have a surrogate key" with "we don't need the natural key" — both usually belong in the table.

### 1.3 Additive vs. semi-additive vs. non-additive measures
Fact table measures should be additive (summable across all dimensions) wherever possible — this is what makes a star schema fast and simple in any BI tool. Flags or indicators intended to be aggregated (e.g., "did this event have property X") should be stored as an integer 0/1, not a true Boolean — most BI engines (including DAX) cannot natively `SUM()` a Boolean, forcing slower, more error-prone filtered-count patterns instead. Ratio measures (e.g., a rate or percentage) should never be pre-stored — compute them as a measure/expression over additive components so they recalculate correctly under any filter context.

### 1.4 Degenerate dimensions and counter columns
A constant "count = 1" column on a fact table (sometimes called a degenerate counter) is standard Kimball practice even when it appears redundant with `COUNT(*)`/`COUNTROWS()`. Its value is consistency: if any other measure on the same fact table (e.g., a flag) is aggregated via `SUM()`, using `SUM()` on a constant-1 column for row counts keeps every KPI on the same aggregation pattern, which is more robust to filter context changes, relationship direction, and future schema evolution (e.g., added bridge tables) than mixing `COUNT`/`COUNTROWS` with `SUM` in the same ratio.

### 1.5 Slowly Changing Dimension (SCD) type selection
Default to Type 1 (overwrite, no history) unless there is a stated business need to preserve historical attribute values as they were at the time of the fact event. Do not introduce Type 2 (versioned rows with effective dates) speculatively — it adds real complexity (surrogate key churn, effective-dated joins) that should only be paid for when a specific, evidenced business question requires it (e.g., "what was this attribute's value at the time of the transaction, even if it has since changed"). When defaulting to Type 1, explicitly name the risk being accepted (e.g., "if X is ever reassigned, historical data will reflect the new value, not the value at time of event") rather than leaving it implicit.

### 1.6 Star schema over snowflake
Prefer a flat dimension (all descriptive attributes in one table) over normalizing into multiple related dimension tables (snowflaking), even when a column is technically a many-to-one rollup of another (e.g., a governed grouping/category column sitting alongside a more granular source value in the same dimension table). Snowflaking adds join complexity with little benefit in a BI semantic model, unless the sub-dimension is genuinely shared/conformed across multiple fact tables.

### 1.7 Structure only where evidence demands it
Do not add columns, tables, or complexity — even "for later" or "just in case" — unless a specific requirement in the BRD, Discovery Framework, or explicit human decision justifies it. Placeholder/stub columns for anticipated future phases should generally be omitted entirely rather than left in as unused fields; they can be added back when the phase that needs them actually arrives. This applies as much to review recommendations as to the draft itself — do not recommend adding structure the human hasn't evidenced a need for, even if it seems technically "more correct" in the abstract.

### 1.8 Conformed dimensions
When multiple fact tables will eventually share a dimension (e.g., a Date or Centre dimension reused across phases), design that dimension to be reusable and consistent across those future facts, rather than scoped narrowly to only the current fact table's needs — but only when reuse is a stated or clearly foreseeable requirement, not speculatively.

---

## Part 2 — Microsoft Fabric / Power BI Semantic Model Best Practices

### 2.1 Model-first design in a governed backend
When the backend is Microsoft Fabric (Lakehouse/Warehouse), dimension tables — including generated ones like a calendar/date dimension — should be built upstream in Fabric as governed tables, not generated inside Power BI (e.g., via DAX `CALENDARAUTO()`), even though DAX generation is technically simpler. This keeps every dimension at the same governance tier, reusable across multiple reports/semantic models sharing the same Fabric backend, rather than having one dimension be invisible to Fabric governance while the others are properly sourced.

### 2.2 Relationship cardinality and direction
Standard star schema relationships should be one-to-many, single-direction, from each dimension table to the fact table. Avoid bidirectional filtering or many-to-many relationships unless there is a specific, evidenced modeling need (e.g., a genuine bridge table for a many-to-many business relationship) — these are a common source of ambiguous or incorrect filter propagation in Power BI.

### 2.3 Data types for join keys and measures
Join keys (surrogate keys, especially date keys) should use simple, stable types — commonly an integer `YYYYMMDD` format for date keys — for reliable joins and easy human debugging/QA, separate from the actual typed `Date` column used for date-based calculations and visuals. Measures intended for aggregation must use types the DAX engine can natively aggregate (integers for summable flags/counters, not Boolean or text).

### 2.4 Hide backend/reconciliation-only fields
Columns that exist purely for backend reconciliation or QA (e.g., a raw source system transaction ID) should be marked hidden in the semantic model rather than exposed in the report's field list, unless there is a stated need for row-level drill-through in the report itself.

### 2.5 Ratio/rate measures as DAX measures, never stored columns
Any percentage or rate calculated from two additive components (e.g., a placement rate) should be implemented as a DAX measure using `DIVIDE()` over the underlying `SUM()` components — never pre-calculated and stored — so that it recalculates correctly under every possible filter/slicer combination a report page might apply.

---

## Part 3 — Microsoft skills-for-fabric Authoring Alignment

Sourced from Microsoft's own `semantic-model-authoring` skill (`skills-for-fabric` repo: `modeling-guidelines.md`, `naming-conventions.md`). These are design-time-relevant principles only — DAX query syntax, trace diagnostics, and performance-tuning content from that same source are execution-time concerns and belong to a future Step 13 Implementation Agent skill, not this reviewer.

Two items below are explicit **corrections/refinements to Part 1**, not additive rules — flagged as such rather than silently merged, since they materially changed decisions in a prior confirmed model (BC SPCA Foster Analysis v1.1).

### 3.1 CORRECTION to 1.2 — No surrogate key on fact tables
Generic Kimball guidance (1.2) treats a fact-table surrogate key as routine. Microsoft's Fabric/Power BI-specific guidance is stricter and takes precedence in this environment: **do not create a surrogate primary key on a fact table.** Nothing ever relates *to* a fact table's own key — dimensions relate *into* the fact via their FKs — so a fact-level surrogate key is pure memory overhead in a column-store (VertiPaq) engine, with no join benefit. The natural/degenerate key from the source system (e.g., a source transaction ID) is sufficient on its own to guarantee grain uniqueness and support reconciliation/QA. When reviewing a fact table design, ask whether a proposed surrogate fact key is doing any real join work — if not, recommend dropping it in favor of the natural key alone.

### 3.2 CORRECTION to naming convention — No `Fact_`/`Dim_` prefixes
Microsoft's naming convention explicitly forbids technical prefixes: no `Fact`, `Dim`, `FACT_`, `DIM_`, `STG_` in table names. Use plain business-friendly names instead — **plural** for fact tables (e.g., `Animal Intakes`, `Sales`, `Orders`), **singular** for dimension tables (e.g., `Animal`, `Centre`, `Product`, `Customer`). This applies to build-ready table names (what will actually exist in the Fabric/Power BI model); architect-facing design documentation may still group tables under "Fact Table Definitions" / "Dimensional Model Definitions" as section headers for clarity, but the table names themselves inside those sections should follow this convention. Also applies to columns and measures: readable casing with spaces, no `CamelCase`/`snake_case`/`UPPER_CASE`, spell out abbreviations unless universally understood in the business domain (YTD, MTD, QTD, etc. are fine).

### 3.3 Explicit measures, hidden base columns
Always use explicit DAX measures rather than relying on a report author's implicit aggregation of a raw column. When a measure aggregates a base fact column (e.g., a measure `Intake Count` defined as `SUM('Animal Intakes'[IntakeCount])`), the base column itself should be marked hidden in the model — the measure is the user-facing interface, not the underlying column. When reviewing a fact table's measures, check that every additive column intended for aggregation has (or will have) a corresponding named, described DAX measure, and flag the base column for hiding once built.

### 3.4 Data type and summarization discipline
- Use `Int64` for keys/identifiers, `Decimal` (never `Double`) for currency or precise numeric values — `Double` causes rounding errors and compression/performance problems in VertiPaq.
- Set `SummarizeBy = None` on numeric columns that are not meant to be aggregated — e.g., a `DateKey` (YYYYMMDD integer), `Year`, `Month` number, or a postal code. Without this, Power BI defaults to offering `SUM`/`AVERAGE` on these columns, which is meaningless and a common source of report-author error.
- Hide foreign key columns on the fact table (the "many" side of every relationship) — they exist for the model's join logic, not for direct report use.

### 3.5 Lean models, memory awareness
Every column costs memory whether or not it is ever used in a report — this reinforces Part 1.7 ("structure only where evidence demands it") with a concrete mechanism, not just a design philosophy. High-cardinality columns (GUIDs, raw transaction IDs, unsplit DateTime, composite string keys) are the largest memory consumers. When reviewing a draft, treat "does this column justify its memory cost" as a real question for any high-cardinality candidate column, not only a scope-creep question.

### 3.6 Composite keys are unsupported, not just discouraged
Microsoft's guidance states composite keys are **not supported** in Power BI relationships, not merely a bad practice. This is a hard technical constraint, not a style preference — it directly reinforces the grain-is-a-sentence principle in 1.1: a fact table's uniqueness must come from a single natural/surrogate key column, never from a combination of FK columns, because the model literally cannot relate on a composite key even if the design called for it.

---

## Further Reading

These are optional references for a human reviewer or junior user who wants to go deeper into a specific principle above. They are not required for this skill's reasoning to function, and this skill does not depend on fetching them.

- Kimball Group — Dimensional Modeling Techniques: https://www.kimballgroup.com/data-warehouse-business-intelligence-resources/kimball-techniques/
- Kimball Group — Kimball's 10 Essential Rules of Dimensional Modeling: https://www.kimballgroup.com/2009/05/the-10-essential-rules-of-dimensional-modeling/
- Microsoft Learn — Star schema and the importance for Power BI: https://learn.microsoft.com/en-us/power-bi/guidance/star-schema
- Microsoft Learn — Relationship guidance for Power BI: https://learn.microsoft.com/en-us/power-bi/guidance/relationships-nfl
- Microsoft Learn — DAX best practices: https://learn.microsoft.com/en-us/power-bi/guidance/dax-coding-standards
- Microsoft Fabric documentation — Lakehouse/Warehouse modeling: https://learn.microsoft.com/en-us/fabric/data-warehouse/
- Microsoft skills-for-fabric — semantic-model-authoring skill (source for Part 3): https://github.com/microsoft/skills-for-fabric/tree/main/skills/semantic-model-authoring
- Power BI Semantic Model Authoring skill overview: https://learn.microsoft.com/en-us/power-bi/developer/agentic/semantic-model-authoring-skill-overview
